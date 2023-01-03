---
title: Security Onion
description: Security Onion
published: true
date: 2022-08-30T04:38:46.749Z
tags: Server, SEIM
editor: markdown
dateCreated: 2022-08-30T04:27:17.152Z
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
Copy the default directory to the local directory as changes done in default are not persistant
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
`so-firewall includehost netflow 192.168.0.0/16`

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
`sudo docker exec -i so-filebeat filebeat setup modules -pipelines -modules netflow -c /usr/share/filebeat/module-setup.yml`

then restart the container
`sudo so-filebeat-restart`


## Link Security Onion to Alienvault
# [AlienVault-OTX](https://docs.securityonion.net/en/2.3/alienvault-otx.html#alienvault-otx "Permalink to this headline")

We can easily pull in [Alienvault OTX](https://otx.alienvault.com/) pulses into Security Onion and have [Zeek](https://docs.securityonion.net/en/2.3/zeek.html#zeek) utilize them for the [Intel Framework](https://docs.zeek.org/en/master/frameworks/intel.html) by leveraging [Stephen Hosom](https://github.com/hosom/bro-otx)’s work with Alienvault OTX integration.

**Warning**
 - Please keep in mind SecurityOnion does not officially support the use of this script, so installation is at your own risk. Also note that some users have reported issues with the OTX feeds causing [Zeek](https://docs.securityonion.net/en/2.3/zeek.html#zeek) to crash.

## [Installation](https://docs.securityonion.net/en/2.3/alienvault-otx.html#installation "Permalink to this headline")

In order to begin, we will need to make sure we satisfy a few prerequisites:

**Alienvault OTX API key** - can be obtained for free at: [https://otx.alienvault.com](https://otx.alienvault.com/)

**Security Onion standalone/sensor** (running [Zeek](https://docs.securityonion.net/en/2.3/zeek.html#zeek))

**External internet access** - to retrieve updated pulses ([https://otx.alienvault.com/api/v1/pulses/subscribed](https://otx.alienvault.com/api/v1/pulses/subscribed))

Download the installation script:

`wget https://raw.githubusercontent.com/weslambert/securityonion-otx/master/securityonion-otx`

![[SecurityOnion_Alienvault_Screenshot_Setup.png]]
Run the script:

`sudo bash securityonion-otx`
**Note** You will need your OTX API code for this step

![[SecurityOnion-Screenshot_setup_Alienvault.png]]
![[Alienvault_SecurityOnion_Setup_1.png]]
I recieved an error for Zeek not starting. I did a restart and It came right up. The error message indicated to check `/nsm/zeek/logs/current/reporter.log` for clues

I did this by `cat /nsm/zeek/logs/current/reporter.log`


After using the above script, `/opt/so/conf/zeek/policy/custom/zeek-otx/` will contain all necessary files including `otx.dat` where all pulses will be populated.

We can test our configuration by adding another piece of intel to the end of `/opt/so/conf/zeek/policy/custom/zeek-otx/otx.dat`. For example:

google.com[literal tab]Intel::DOMAIN[literal tab]Test-Google-Intel[literal tab]https://google.com[literal tab]T

As long as our syntax is correct, we should not need to restart Zeek. We can check for errors in `/nsm/zeek/logs/current/reporter.log`.

Let’s see if we can get an intel hit by doing the following:

`curl google.com`

Next, we need to check `/nsm/zeek/logs/current/intel.log` for entries relating to our indicator:

`grep google /nsm/zeek/logs/current/intel.log`

We should have received a Zeek Notice as well, so let’s check that:

`grep google /nsm/zeek/logs/current/notice.log`

After successful testing, we can remove our addition from `/opt/so/conf/zeek/policy/custom/zeek-otx/otx.dat` or just run `sudo python3 /usr/sbin/zeek-otx.py` again.

By default, pulses will be retrieved on an hourly basis. To change this to a different value, simply alter the interval in `/etc/cron.d/zeek-otx`.


## [Wazuh](https://docs.securityonion.net/en/2.3/wazuh.html#wazuh "Permalink to this headline")

## [Description](https://docs.securityonion.net/en/2.3/wazuh.html#description "Permalink to this headline")

From [https://wazuh.com/](https://wazuh.com/):

> Wazuh is a free, open source and enterprise-ready security monitoring solution for threat detection, integrity monitoring, incident response and compliance.

## [Usage](https://docs.securityonion.net/en/2.3/wazuh.html#usage "Permalink to this headline")

Security Onion utilizes Wazuh as a Host Intrusion Detection System (HIDS) on each of the Security Onion nodes.

The Wazuh components include:

`manager` - runs inside of `so-wazuh` Docker container and performs overall management of agents

`API` - runs inside of `so-wazuh` Docker container and allows for remote management of agents, querying, etc.

`agent` - runs directly on each host and monitors logs/activity and reports to `manager`

The Wazuh API runs at TCP port `55000` locally, and currently uses the default credentials of `user:foo` and `password:bar` for authentication. Keep in mind, the API port is not exposed externally by default. Therefore, firewall rules need to be in place to reach the API from another location other than the Security Onion node on which the targeted Wazuh manager is running.

Since the manager runs inside a Docker container, many of the Wazuh binaries that you might want to run will need to be run inside the Docker container. For example, to run `agent_upgrade`:

sudo so-wazuh-agent-upgrade

## [Configuration](https://docs.securityonion.net/en/2.3/wazuh.html#configuration "Permalink to this headline")

The main configuration file for Wazuh is `/opt/so/conf/wazuh/ossec.conf`.

## [Syslog](https://docs.securityonion.net/en/2.3/wazuh.html#syslog "Permalink to this headline")

If you want to send Wazuh logs to an external syslog collector, please see the [Syslog Output](https://docs.securityonion.net/en/2.3/syslog-output.html#syslog-output) section.

## [Active Response](https://docs.securityonion.net/en/2.3/wazuh.html#active-response "Permalink to this headline")

Sometimes, Wazuh may recognize legitimate activity as potentially malicious and engage in Active Response to block a connection. This may result in unintended consequences such as blocking of trusted IPs. To prevent this from occurring, you can add your IP address to a safe list and change other settings in `/opt/so/conf/wazuh/ossec.conf` in the `<!-- Active response -->` section. [so-allow](https://docs.securityonion.net/en/2.3/so-allow.html#so-allow) does this for you automatically when you allow analyst connections.

## [Email](https://docs.securityonion.net/en/2.3/wazuh.html#email "Permalink to this headline")

If you want [Wazuh](https://docs.securityonion.net/en/2.3/wazuh.html#wazuh) to send email, you can modify `/opt/so/conf/wazuh/ossec.conf` as follows:

<global>
<email_notification>yes</email_notification>
<email_to>YourUsername@YourDomain.com</email_to>
<smtp_server>YourMailRelay.YourDomain.com</smtp_server>
<email_from>ossec@YourDomain.com</email_from>
<email_maxperhour>100</email_maxperhour>
</global>

Then restart [Wazuh](https://docs.securityonion.net/en/2.3/wazuh.html#wazuh):

sudo so-wazuh-restart

You can specify the severity of an event for which [Wazuh](https://docs.securityonion.net/en/2.3/wazuh.html#wazuh) will send email alerts by specifying an appropriate value for `email_alert_level` in `/opt/so/conf/wazuh/ossec.conf`. If you notice `email_alert_level` is not being respected for a certain rule, it may be that the option is overridden by `<options>alert_by_email</options>` being set for a rule. You can modify this behavior in `/opt/so/conf/wazuh/rules/local_rules.xml`.

You can also find an explanation of the alert levels at [https://www.ossec.net/docs/manual/rules-decoders/rule-levels.html](https://www.ossec.net/docs/manual/rules-decoders/rule-levels.html).

## [Tuning Rules](https://docs.securityonion.net/en/2.3/wazuh.html#tuning-rules "Permalink to this headline")

### [New Rules](https://docs.securityonion.net/en/2.3/wazuh.html#new-rules "Permalink to this headline")

You can add new rules in `/opt/so/rules/hids/local_rules.xml`.

### [Modify Existing Rules](https://docs.securityonion.net/en/2.3/wazuh.html#modify-existing-rules "Permalink to this headline")

You can modify existing rules by copying the rule to `/opt/so/rules/hids/local_rules.xml`, making your changes, and adding `overwrite="yes"` as shown at [https://documentation.wazuh.com/current/user-manual/ruleset/custom.html#changing-an-existing-rule](https://documentation.wazuh.com/current/user-manual/ruleset/custom.html#changing-an-existing-rule). To suppress a Wazuh alert, you can add the rule and include `noalert="1"` in the `rule` section.

The overall process would be as follows:

1.  First, find the existing rule in `/opt/so/rules/hids/ruleset/rules/`.
2.  Copy the rule to `/opt/so/rules/hids/local_rules.xml`.
3.  Put the rule inside `<group> </group>` tags and give it a name.
4.  Update the `<rule>` section to include `noalert="1"` along with `overwrite="yes"`.
5.  Finally, restart Wazuh with `sudo so-wazuh-restart`.

Here is an example to suppress “Windows Logon Success” and “Windows User Logoff” alerts:

> <group name="customrules,">
>   <rule id="60106" level="3" noalert="1" overwrite="yes">
>     <if_sid>60103</if_sid>
>     <field name="win.system.eventID">^528$|^540$|^673$|^4624$|^4769$</field>
>     <description>Windows Logon Success</description>
>     <options>no_full_log</options>
>     <mitre>
>       <id>T1078</id>
>     </mitre>
>     <group>authentication_success,pci_dss_10.2.5,gpg13_7.1,gpg13_7.2,gdpr_IV_32.2,hipaa_164.312.b,nist_800_53_AU.14,nist_800_53_AC.7,tsc_CC6.8,tsc_CC7.2,tsc_CC7.3,</group>
>   </rule>
> 
>   <rule id="60137" level="3" noalert="1" overwrite="yes">
>     <if_sid>60103</if_sid>
>     <field name="win.system.eventID">^538$|^551$|^4634$|^4647$</field>
>     <description>Windows User Logoff</description>
>     <options>no_full_log</options>
>     <group>pci_dss_10.2.5,gdpr_IV_32.2,hipaa_164.312.b,nist_800_53_AU.14,nist_800_53_AC.7,tsc_CC6.8,tsc_CC7.2,tsc_CC7.3,</group>
>   </rule>
> </group>

Note

This will not remove existing alerts that were generated before applying the new rule. Also note that this only suppresses the alert and not the underlying log.

### [Child Rules](https://docs.securityonion.net/en/2.3/wazuh.html#child-rules "Permalink to this headline")

In addition to overwriting rules, another option is to add child rules using `if_sid`. In this example, suppose you are receiving Wazuh alerts for `PAM: Login session closed` and want to stop receiving those alerts for a particular user account.

Let’s start by using `ossec-logtest` with a default configuration and pasting in a sample log:

[doug@securityonion ~]$ sudo docker exec -it so-wazuh /var/ossec/bin/ossec-logtest
2022/02/24 17:52:49 ossec-testrule: INFO: Started (pid: 2298).
ossec-testrule: Type one log per line.

Feb 24 17:46:19 securityonion sshd[37140]: pam_unix(sshd:session): session closed for user doug


**Phase 1: Completed pre-decoding.
       full event: 'Feb 24 17:46:19 securityonion sshd[37140]: pam_unix(sshd:session): session closed for user doug'
       timestamp: 'Feb 24 17:46:19'
       hostname: 'securityonion'
       program_name: 'sshd'
       log: 'pam_unix(sshd:session): session closed for user doug'

**Phase 2: Completed decoding.
       decoder: 'pam'
       dstuser: 'doug'

**Phase 3: Completed filtering (rules).
       Rule id: '5502'
       Level: '3'
       Description: 'PAM: Login session closed.'
**Alert to be generated.

This shows us the rule that would fire and its Rule id of `5502`. Now let’s add the following rule to `/opt/so/rules/hids/local_rules.xml`:

<rule id="100002" level="1">
  <if_sid>5502</if_sid>
  <match>doug</match>
  <description>ignore logins from doug</description>
</rule>

Finally, let’s re-run `ossec-logtest`:

[doug@securityonion ~]$ sudo docker exec -it so-wazuh /var/ossec/bin/ossec-logtest
2022/02/24 17:54:26 ossec-testrule: INFO: Started (pid: 2305).
ossec-testrule: Type one log per line.

Feb 24 17:46:19 securityonion sshd[37140]: pam_unix(sshd:session): session closed for user doug


**Phase 1: Completed pre-decoding.
       full event: 'Feb 24 17:46:19 securityonion sshd[37140]: pam_unix(sshd:session): session closed for user doug'
       timestamp: 'Feb 24 17:46:19'
       hostname: 'securityonion'
       program_name: 'sshd'
       log: 'pam_unix(sshd:session): session closed for user doug'

**Phase 2: Completed decoding.
       decoder: 'pam'
       dstuser: 'doug'

**Phase 3: Completed filtering (rules).
       Rule id: '100002'
       Level: '1'
       Description: 'ignore logins from doug'
**Alert to be generated.

This shows us that Wazuh no longer fires rule `5502` but now fires our new alert. Once your changes are complete, restart Wazuh with `sudo so-wazuh-restart`.

## [Apparmor DENIED Alerts](https://docs.securityonion.net/en/2.3/wazuh.html#apparmor-denied-alerts "Permalink to this headline")

If you’re running on Ubuntu, then you most likely get Wazuh HIDS alerts for `Apparmor DENIED`. In most cases, this is due to telegraf (part of [Grafana](https://docs.securityonion.net/en/2.3/grafana.html#grafana)) trying to use `ptrace`. To silence these alerts, you might want to add a child rule as shown in the section above. `Apparmor DENIED` is Wazuh sid `52002` so that would go in the `if_sid` section. We could then use a regular expression to look for `operation="ptrace"` and `comm="telegraf"` but allow any digit value in the `pid` field:

<rule id="100002" level="0">
  <if_sid>52002</if_sid>
  <description>ignore apparmor denied messages from telegraf</description>
  <regex>apparmor="DENIED" operation="ptrace" profile="docker-default" pid=\d+ comm="telegraf"</regex>
</rule>

We could add this to `/opt/so/rules/hids/local_rules.xml` and then restart Wazuh with `sudo so-wazuh-restart`.

## [Adding Agents](https://docs.securityonion.net/en/2.3/wazuh.html#adding-agents "Permalink to this headline")

Navigate to the Downloads page in [Security Onion Console (SOC)](https://docs.securityonion.net/en/2.3/soc.html#soc) and download the appropriate Wazuh agent for your endpoint. This will ensure that you get the correct version of Wazuh. If your endpoint is not listed there, you can check the Wazuh website at [https://documentation.wazuh.com/3.13/installation-guide/packages-list/index.html](https://documentation.wazuh.com/3.13/installation-guide/packages-list/index.html).

Warning

It is important to ensure that you download the agent that matches the version of your Wazuh server. For example, if your Wazuh server is version 3.13.1, then you will want to deploy Wazuh agent version 3.13.1.

You can verify the version of your current Wazuh server using the following command:

sudo docker exec -it so-wazuh dpkg -l |grep wazuh

Once you’ve installed the Wazuh agent on the host(s) to be monitored, then perform the steps defined here:

[https://documentation.wazuh.com/3.13/user-manual/registering/command-line-registration.html](https://documentation.wazuh.com/3.13/user-manual/registering/command-line-registration.html)

Please keep in mind that when you run `manage_agents` you will need to do so inside the `so-wazuh` container like this:

sudo so-wazuh-agent-manage

You also may need to run [so-allow](https://docs.securityonion.net/en/2.3/so-allow.html#so-allow) to allow traffic from the IP address of your Wazuh agent(s).

## [Maximum Number of Agents](https://docs.securityonion.net/en/2.3/wazuh.html#maximum-number-of-agents "Permalink to this headline")

Security Onion is configured to support a maximum number of `14000` Wazuh agents reporting to a single Wazuh manager.

## [Automated Deployment](https://docs.securityonion.net/en/2.3/wazuh.html#automated-deployment "Permalink to this headline")

If you would like to automate the deployment of Wazuh agents, the Wazuh server includes `ossec-authd`. You can read more about `ossec-authd` at [https://documentation.wazuh.com/3.13/user-manual/reference/daemons/ossec-authd.html](https://documentation.wazuh.com/3.13/user-manual/reference/daemons/ossec-authd.html).

When using `ossec-authd`, be sure to add a firewall exception for agents to access port `1515/tcp` on the Wazuh manager node by running [so-allow](https://docs.securityonion.net/en/2.3/so-allow.html#so-allow) and choosing the `r` option.

## [API](https://docs.securityonion.net/en/2.3/wazuh.html#api "Permalink to this headline")

The Wazuh API runs on port `55000` and requires a user to be created for access. To add a new user, run `so-wazuh-user-add` as follows (replacing `newuser` with the actual username you’d like to create):

sudo so-wazuh-user-add newuser

When prompted, provide a password for the new user. Once the user has been added, then restart Wazuh:

sudo so-wazuh-restart

Once restarted, try accessing the API locally from the node using the newly created user and password:

curl -k -u newuser:password https://localhost:55000

You should receive a message similar to the following indicating success:

{"error":0,"data":{"msg":"Welcome to Wazuh HIDS API","api_version":"v3.13.1","hostname":"securityonion-is-the-coolest","timestamp":"Wed Feb 02 2022 13:09:03    GMT+0000 (UTC)"}}

If you receive a `401` (Unauthorized) error message, double-check the credentials or try running `sudo so-wazuh-user-passwd` if necessary. You can also check the `user` file inside the Docker container:

sudo docker exec -it so-wazuh cat /var/ossec/api/configuration/auth/user

## [Diagnostic Logging](https://docs.securityonion.net/en/2.3/wazuh.html#diagnostic-logging "Permalink to this headline")

Wazuh diagnostic logs are in `/nsm/wazuh/logs/`. Depending on what you’re looking for, you may also need to look at the [Docker](https://docs.securityonion.net/en/2.3/docker.html#docker) logs for the container:

sudo docker logs so-wazuh

## [More Information](https://docs.securityonion.net/en/2.3/wazuh.html#more-information "Permalink to this headline")

See also

For more information about Wazuh, please see [https://documentation.wazuh.com/3.13/](https://documentation.wazuh.com/3.13/).