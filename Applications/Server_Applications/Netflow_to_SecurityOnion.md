---
title: Netflow to Security Onion
description: Security Onion
dateCreated: 2022-08-30T04:27:17.152Z
published: true
editor: markdown
tags:
  - Server
  - SEIM
dateModified: 
---
# [Security Onion](https://securityonionsolutions.com/)

## Netflow to Security Onion

[youtube Video](https://www.youtube.com/watch?v=ew5gtVjAs7g)

STEP 1

Update Minion File

`sudo su`

`cd /opt/so/saltstack/local/pillar/minions`

`ls` find your /*.filebeat./*.sls mine is named seconion_standalone.sls

`vi seconion_standalone.sls`

press insert

```
filebeat:
  third_party_filebeat:
    modules:
      netflow:
        log:
          enabled: true
          var.netflow_host: 0.0.0.0
          var.netflow_port: 2055
```

press esc and type `:wq`

*Note* if you only want a specific host to push netflow from change 0.0.0.0 (all) to that ip example 192.168.255.254 also if you run a port different than the default netflow port make sure to change 2055 to that no-default port number

Step 2

Update Docker Config

run the below command and see the container location and ports

`docker ps | grep filebeat`

*Note* By default the filebeat container is not open to port 2055 so we will have to change that

cd into the local salt stack directory

`cd /opt/so/saltstack/local/salt/filebeat`

Copy the default directory to the local directory as changes done in default are not persistent

`sudo cp /opt/so/saltstack/default/salt/filebeat/init.sls /opt/so/saltstack/local/salt/filebeat/init.sls`

Change the owner of init.sls to socore

`sudo chown socore:socore /opt/so/saltstack/local/salt/filebeat/init.sls`

Edit the init.sls file

`vi init.sls`

press insert

look for the following

```
    126    - port_bindings:
    127         - 0.0.0.0:514:514/udp
    128         - 0.0.0.0:514:514/tcp
    129         - 0.0.0.0:5066:5066/tcp
```

around line 129

add ` - 0.0.0.0:2055:2055/udp`

your port mappings should look somewhat like this now

```
    126     - port_bindings:
    127         - 0.0.0.0:514:514/udp
    128         - 0.0.0.0:514:514/tcp
    129         - 0.0.0.0:2055:2055/udp
    130         - 0.0.0.0:5066:5066/tcp
```

press ESC

type `:wq`

press enter

Now we need to tell salt to update the filebeat mappings

`sudo salt-call state.apply filebeat`

After complete verify the container is accepting connections on port 2055

`docker ps | grep 2055`

Step 3

Update Firewall Config

Add the hostgroup

`so-firewall addhostgroup netflow`

add the portgroup

`so-firewall addportgroup netflow`

add the hosts allowed to send netflow

`so-firewall includehost netflow 0.0.0.0/0`

add netflow port

`so-firewall addport netflow udp 2055`

update firewall config 

`cd /opt/so/saltstack/local/pillar/minions`

`vi **yourminionfile.sls` mine is called seconion_standalone.sls

go to the end of the file and press `insert` and ctrl+c/v the following

```
firewall:
  assigned_hostgroups:
    chain:
      DOCKER-USER:
        hostgroups:
          netflow:
            portgroups:
              - portgroups.netflow
      INPUT:
        hostgroups:
          netflow:
            portgroups:
              - portgroups.netflow
```

press `Esc` then type `:wq`

now time to apply the rules

`sudo salt-call state.apply firewall`

confirm with

`iptables --list -n | grep 2055`

Step 4

Update the Logstash pipeline

now apply the logstash config file to filebeats

(OLD VERSION)

`sudo docker exec -i so-filebeat filebeat setup modules -pipelines -modules netflow -c /usr/share/filebeat/module-setup.yml`

(2.3 and Newer) [see this link for details](https://github.com/Security-Onion-Solutions/securityonion/discussions/8745)

`sudo docker exec -it so-filebeat filebeat setup modules --pipelines --modules netflow -M "netflow.log.enabled=true" -c /usr/share/filebeat/module-setup.yml`

OR add the configuration to the piller

`

third_party_filebeat:

  modules:

    netflow:

      log:

        enabled: true

`

then run `salt-call state.apply common` and run `so-filebeat-module-setup`

then restart the container

`sudo so-filebeat-restart`