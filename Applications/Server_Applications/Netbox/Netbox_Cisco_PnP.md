---
title: 
description: 
dateCreated: 
published: 
editor: markdown
tags: 
dateModified: 
---
# How to Auto Provision Devices with NetBox and Cisco PnP Provision

Most administrators still integrate new network devices into the network with the help of the famous blue cable, i.e. console access.

But for lazy administrators all manufacturers offer so-called Zero Touch Provision (ZTP) options, utilizing TFTP servers or the TR069 protocol. Cisco came up with the more modern solution called Cisco Plug-and-Play (Cisco PnP) protocol. With the help of NetBox as source of truth, a DHCP server and a small python application you can set up an environment to auto-provision your Cisco devices.

The setup also is the basis for a setup to manage your network completely based on the NetBox configuration.

## Cisco PnP

Cisco describes the modern PnP protocol as an alternative to the old-fashioned to the ZTP process utilizing a TFTP server. The complete setup is implemented i.e. in a Cisco DNA server.

Basically a new device discovers a central provision server by DHCP or DNS or other methods. In our setup I will use the ISC Kea DHCP server to provide the information where to find the provisioning server.

Then the new device connects the server with a HELLO request to check if the server is available. If the server answers with a 200 OK the device accepts the server and periodically requests a WORK-REQUEST from the server. The server answers by sending a new configuration or an update to the operation system.

Using this setup of the central PnP server and the periodic WORK-REQUESTS from the devices, we can set up a complete centralized system to manage all the devices in the network. A central NetBox instance as a Source-of-Truth that provides the configuration of the devices via config_templates is the central component of the setup.

In this article I want to describe the complete setup of such a system of a DHCP server, NetBox and a python flask application as PnP server to achieve the task of a zero touch provisioning of the devices and subsequent centralized management via NetBox.

## The DHCP Server

If a new Cisco device is booted without any configuration it requests an IP address via the DHCP protocol. The DHCP server can be utilized to pass additional information to the new device. Especially in option 43 the server can provide information about the PnP server.

The dhcp setup is described in [another article](https://sys4.de/blog/kea-veos) (ISC Kea DHCP.)

If the DHCP client sends the text cisconpnp in the request, the DHCP server adds the information about the PnP server protocol and IP address to the answer.

## The PnP Server

In the next step we will set up a PnP server that delivers the configuration to the new device. We use a simple flask application. This setup is based on the work of dmfigol on [github](https://github.com/dmfigol/cisco-pnp-server).

Please note that I am not a programmer and there are a lot of improvements to the application. As soon as I find time I will upload it to github and take care of the merge requests.

### Application Setup

I will describe the flask application in detail on the following sections, so anyone can send improvements. First we have to set up the program:

```
import re
from flask import Flask, request, send_from_directory, render_template, Response, send_file
from pathlib import Path

import requests
import xmltodict

import pynetbox
import json

app = Flask(__name__, template_folder="templates")
current_dir = Path(__file__)

session = requests.Session()
session.verify = False
```

Next we connect to the NetBox to manage our devices:

```
nb = pynetbox.api(
    'https://192.0.2.16',
    token='12345678'
)
nb.http_session = session
```

The first request of the new device is a HELLO that must be answered with a 200 OK.

```
@app.route('/pnp/HELLO')
def pnp_hello():
    return '', 200
```

All follow up requests of the devices will be POST to the WORK-REQUEST route. Here our work starts.

The idea is to check NetBox, if the device exists and what is the status of the device. Devices are identified by their serial number. If the device does not yet exist, i.e. it is a completely new device, the app will add the new device to NetBox and set its status to provision. Since the device tells the app in the request about its serial number and its device type, it is easy to add it to NetBox. In the simple app I use a fixed device name staging to add the device. More sophisticated setups are welcome.

```
@app.route('/pnp/WORK-REQUEST', methods=['POST'])
def pnp_work_request():
  print(request.data)
  data = xmltodict.parse(request.data)
  correlator_id = data['pnp']['info']['@correlator']
  udi = data['pnp']['@udi']
  udi_match = re.match(r'PID:(?P<product_id>[\w-]+),VID:(?P<hw_version>\w+),SN:(?P<serial_number>\w+)', udi)
  serial_number = udi_match.group('serial_number')
  product_id = udi_match.group('product_id')
  #
  # If the device does not exist in netbox, add it:
  #
  if nb.dcim.devices.count(serial=serial_number) == 0:
    print ("Device not in netbox. Adding ...")
    device = nb.dcim.devices.create(
      name = 'staging',
      serial = serial_number,
      status='planned',
      site = nb.dcim.sites.get(name='home').id,
      device_type=nb.dcim.device_types.get(model=product_id).id,
      role=nb.dcim.device_roles.get(name='switch').id
    )
    return '', 200
  else:
    (...)
```

Now the administrator can configure the new device in NetBox, i.e. setting up the correct device name, the management IP address and VLAN and finally assigning a config template to the device. In the last step the administrator sets the status to staged. Only with this status the app will proceed.

```
  else:
    device = nb.dcim.devices.get(serial=serial_number)
    if device.status.value == 'staged':
      config_filename = device.serial + '.cfg'
      jinja_context = {
        "device": device
      }
      config_text = render_template('provision.jinja', **jinja_context)
      url = "http://192.0.2.16/api/dcim/devices/" + str(device.id) + "/render-config/"
      headers = {"Content-Type": "application/json", "Authorization": "Token 12345678"}
      response = requests.post(url, headers=headers)
      with open("configs/"+config_filename, "w") as f:
        f.write(response.json().get("content"))
      jinja_context = {
        "udi": udi,
        "correlator_id": correlator_id,
        "config_filename": config_filename,
      }
      result_data = render_template('config-upgrade.xml', **jinja_context)
      return Response(result_data, mimetype='text/xml')
    else:
      #
      # Add code for the every day opertation here.
      #
      return '',200
```

Of course the app also has to take care of the answer of the device. After the device finishes the WORK-REQUEST it sends a WORK-RESPONSE as an answer to the PnP server. The server just acknowledges the reply:

```
@app.route('/pnp/WORK-RESPONSE', methods=['POST'])
def pnp_work_response():
    print(request.data)
    data = xmltodict.parse(request.data)
    correlator_id = data['pnp']['response']['@correlator']
    udi = data['pnp']['@udi']
    jinja_context = {
        "udi": udi,
        "correlator_id": correlator_id,
    }
    #
    # Better: Update state in netbox to active
    #
    result_data = render_template('bye.xml', **jinja_context)
    return Response(result_data, mimetype='text/xml')
```

## Working Example

The config template in NetBox can be written in Jinja. The following template is inspired by the demonstration of the template [capabilities](https://netboxlabs.com/blog/how-to-generate-device-configurations-with-netbox).

```
hostname {{ device.name }}
ip domain-name example.com

# Run LLDP for later automation
lldp run
service password-encryption

# Setup local user
enable password 0 cisco
username cisco privilege 15 password 0 cisco
aaa new-model
aaa authentication login default local
aaa authentication enable default enable
aaa authorization exec default local if-authenticated 

crypto key generate rsa general-keys modulus 2048
ip ssh version 2
ip scp server enable
no ip http server
no ip http secure-server

no spanning-tree portfast bpduguard
no ip domain-lookup

interface vlan 1
shutdown

{% for vlan in device.site.vlans.all() %}vlan {{vlan.vid }}
{% if true %} name {{ vlan.name }}{% endif %}
{% endfor %}

{%- for interface in device.interfaces.all() %}
{% if interface.mgmt_only == false %}interface {{ interface.name }}
{% if interface.enabled == false %}  shutdown
{% elif interface.enabled == true %}  no shutdown
{%- endif  %}
{% if interface.mode == "access" %}  switchport mode access
switchport access vlan {{ interface.untagged_vlan.vid }}
{%- elif interface.mode == "tagged" %}  switchport mode trunk {%- for vlan in interface.tagged_vlans.all() %}  switchport trunk allowed vlan add {{ vlan.vid }}
{%- endfor %}
{%- elif "tagged-all" in interface.mode %}  switchport mode trunk
  switchport trunk allowed vlan all
{%- else %}
{%- if interface.lag != None %}  channel-group
{% for char in interface.lag.name %}
{%- if char.isdigit() %}{{ char }}
{%- endif %}
{%- endfor %}  mode on
{%- endif %} 
{%- endif %}
{% else %}interface {{ interface.name }}
  description Management VLAN
{% if interface.enabled == false %}  shutdown
{% elif interface.enabled == true %}  no shutdown
{%- endif %}
  ip address {{ device.primary_ip.address.ip }} {{ device.primary_ip.address.netmask }}
{%- endif %}
{%- endfor %}

line con 0
 exec-timeout 0 0
line vty 0 4
 exec-timeout 0 0
 transport input ssh
line vty 5 14
 transport input ssh
line vty 15
```

If you start your DHCP server and the flask application with

```
root@host# flask --app main run --host 192.0.2.16 --port 80
```

and boot your new device you can watch the PnP process the the logs of the device, if you connect to device with the blue cable:

```
Cisco IOS Software, C2960L Software (C2960L-UNIVERSALK9-M), Version 15.2(7)E7, RELEASE SOFTWARE (fc10)
Technical Support: http://www.cisco.com/techsupport
Copyright (c) 1986-2022 by Cisco Systems, Inc.
Jan  6 02:14:44.421: %PNP-6-PNP_BEST_UDI_UPDATE: Best UDI [PID:WS-C2960L-8TS-LL,VID:V02,SN:FCW2234A267] identified via (master-registry)
(...)
Jan  6 02:14:46.148: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/8, changed state to up
Jan  6 02:14:48.108: %LINK-3-UPDOWN: Interface GigabitEthernet0/8, changed state to up
Jan  6 02:14:50.128: %LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to up
(...)
Jan  6 02:15:30.878: %PNPA-DHCP Op-43 Msg: Process state = READY
Jan  6 02:15:30.878: %PNPA-DHCP Op-43 Msg: OK to process message
Jan  6 02:15:30.879: %PNP-6-PNP_DHCP_VALID_PNP_OPTION_NOTIFIED: DHCP valid-PnP option (+5A1D;K4;B2;I192.0.2.16) on interface (Vlan1) no)
Jan  6 02:15:30.879: %PNPA-DHCP Op-43 Msg: _pdoon.1.ntf.don=151
Jan  6 02:15:30.879: %PNPA-DHCP Op-43 Msg: _pdoop.1.org=[A1D;K4;B2;I192.0.2.16]
Jan  6 02:15:30.879: %PNPA-DHCP Op-43 Msg: _pdgfa.1.inp=[K4;B2;I192.0.2.16]
Jan  6 02:15:30.880: %PNPA-DHCP Op-43 Msg: _pdgfa.1.K4.htp=[ transport http ]
Jan  6 02:15:30.880: %PNPA-DHCP Op-43 Msg: _pdgfa.1.B2.s12=[ ipv4 ]
Jan  6 02:15:30.880: %PNPA-DHCP Op-43 Msg: _pdgfa.1.Ix.srv.ip.rm=[ 192.0.2.16 ]
Jan  6 02:15:30.880: %PNPA-DHCP Op-43 Msg: _pdoop.1.ztp=[pnp-zero-touch] host=[] ipad=[192.0.2.16] port=80
Jan  6 02:15:30.880: %PNPA-DHCP Op-43 Msg: _pors.done=1
Jan  6 02:15:30.880: %PNPA-DHCP Op-43 Msg: _pdokp.1.kil=[PNPA_DHCP_OP43] pid=151 idn=[Vlan1]
Jan  6 02:15:32.426: %PNP-6-PNP_DOMAIN_NAME_SET: Domain name (example.org) set on (Vlan1) by (pid=305, pname=PnP Agent Discovery, time=02:1)
Jan  6 02:15:35.912: %RAC-3-RACIPL: DHCP is already running on interface Vlan1
Jan  6 02:15:35.913: AUTOINSTALL: ip address dhcp configured on Vlan1
(...)
Jan  6 02:15:59.426: %PNP-6-HTTP_CONNECTING: PnP Discovery trying to connect to PnP server (http://192.0.2.16:80/pnp/HELLO)
Jan  6 02:16:00.441: %PNP-6-HTTP_CONNECTED: PnP Discovery connected to PnP server (http://192.0.2.16:80/pnp/HELLO)
Jan  6 02:16:00.442: %PNP-6-PNP_PROFILE_CREATED: PnP profile (pnp-zero-touch) created (1/3) by (pid=305, pname=PnP Agent Discovery, time=02)
Jan  6 02:16:01.451: %PNP-6-PNP_RELOAD_INFO_ENCODED: Reload reason (PnP Service Info 2408-power-on) encoded (1/3) by (pid=305, pname=PnP Ag)
Jan  6 02:16:01.453: %PNP-6-PROFILE_CONFIG: PnP Discovery profile pnp-zero-touch configured
Jan  6 02:16:01.453: %PNP-6-PNP_SAVING_TECH_SUMMARY: Saving PnP tech summary (/pnp-tech/pnp-tech-discovery-summary)... Please wait. Do not .
Jan  6 02:16:02.459: %PNP-6-PNP_TECH_SUMMARY_SAVED_OK: PnP tech summary (/pnp-tech/pnp-tech-discovery-summary) saved successfully (elapsed .
Jan  6 02:16:02.459: %PNP-6-PNP_DISCOVERY_DONE: PnP Discovery done successfully (PnP-DHCP-IPv4) profile (pnp-zero-touch) via  (http://192.0)
(...)
Jan  6 02:18:19.941: %SSH-5-ENABLED: SSH 1.99 has been enabled
(...)
Jan  6 02:18:24.338: %LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan10, changed state to up
Jan  6 02:18:26.233: %LINK-5-CHANGED: Interface Vlan1, changed state to administratively down
Jan  6 02:18:26.234: %LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to down
```

After this process, the device has the correct config and is ready to work.