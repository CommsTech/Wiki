---
title: Wazuh on Security Onion
description: 
dateCreated: 
published: 
editor: markdown
tags: 
dateModified: 
---
# [Wazuh on security onion](https://docs.securityonion.net/en/2.3/wazuh.html#wazuh "Permalink to this headline")


> Wazuh is a free, open source and enterprise-ready security monitoring solution for threat detection, integrity monitoring, incident response and compliance.

# [Usage](https://docs.securityonion.net/en/2.3/wazuh.html#usage "Permalink to this headline")

Security Onion utilizes Wazuh as a Host Intrusion Detection System (HIDS) on each of the Security Onion nodes.

The Wazuh components include:

`manager` - runs inside of `so-wazuh` Docker container and performs overall management of agents

`API` - runs inside of `so-wazuh` Docker container and allows for remote management of agents, querying, etc.

`agent` - runs directly on each host and monitors logs/activity and reports to `manager`

The Wazuh API runs at TCP port `55000` locally, and currently uses the default credentials of `user:foo` and `password:bar` for authentication. Keep in mind, the API port is not exposed externally by default. Therefore, firewall rules need to be in place to reach the API from another location other than the Security Onion node on which the targeted Wazuh manager is running.

Since the manager runs inside a Docker container, many of the Wazuh binaries that you might want to run will need to be run inside the Docker container. For example, to run `agent_upgrade`:

sudo so-wazuh-agent-upgrade

# [Configuration](https://docs.securityonion.net/en/2.3/wazuh.html#configuration "Permalink to this headline")

The main configuration file for Wazuh is `/opt/so/conf/wazuh/ossec.conf`.

# [Syslog](https://docs.securityonion.net/en/2.3/wazuh.html#syslog "Permalink to this headline")

If you want to send Wazuh logs to an external syslog collector, please see the [Syslog Output](https://docs.securityonion.net/en/2.3/syslog-output.html#syslog-output) section.

# [Active Response](https://docs.securityonion.net/en/2.3/wazuh.html#active-response "Permalink to this headline")

Sometimes, Wazuh may recognize legitimate activity as potentially malicious and engage in Active Response to block a connection. This may result in unintended consequences such as blocking of trusted IPs. To prevent this from occurring, you can add your IP address to a safe list and change other settings in `/opt/so/conf/wazuh/ossec.conf` in the `<!-- Active response -->` section. [so-allow](https://docs.securityonion.net/en/2.3/so-allow.html#so-allow) does this for you automatically when you allow analyst connections.

# [Email](https://docs.securityonion.net/en/2.3/wazuh.html#email "Permalink to this headline")

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

# [Tuning Rules](https://docs.securityonion.net/en/2.3/wazuh.html#tuning-rules "Permalink to this headline")

## [New Rules](https://docs.securityonion.net/en/2.3/wazuh.html#new-rules "Permalink to this headline")

You can add new rules in `/opt/so/rules/hids/local_rules.xml`.

## [Modify Existing Rules](https://docs.securityonion.net/en/2.3/wazuh.html#modify-existing-rules "Permalink to this headline")

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

## [Child Rules](https://docs.securityonion.net/en/2.3/wazuh.html#child-rules "Permalink to this headline")

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

# [Apparmor DENIED Alerts](https://docs.securityonion.net/en/2.3/wazuh.html#apparmor-denied-alerts "Permalink to this headline")

If you’re running on Ubuntu, then you most likely get Wazuh HIDS alerts for `Apparmor DENIED`. In most cases, this is due to telegraf (part of [Grafana](https://docs.securityonion.net/en/2.3/grafana.html#grafana)) trying to use `ptrace`. To silence these alerts, you might want to add a child rule as shown in the section above. `Apparmor DENIED` is Wazuh sid `52002` so that would go in the `if_sid` section. We could then use a regular expression to look for `operation="ptrace"` and `comm="telegraf"` but allow any digit value in the `pid` field:

<rule id="100002" level="0">
  <if_sid>52002</if_sid>
  <description>ignore apparmor denied messages from telegraf</description>
  <regex>apparmor="DENIED" operation="ptrace" profile="docker-default" pid=\d+ comm="telegraf"</regex>
</rule>

We could add this to `/opt/so/rules/hids/local_rules.xml` and then restart Wazuh with `sudo so-wazuh-restart`.

# [Adding Agents](https://docs.securityonion.net/en/2.3/wazuh.html#adding-agents "Permalink to this headline")

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

# [Maximum Number of Agents](https://docs.securityonion.net/en/2.3/wazuh.html#maximum-number-of-agents "Permalink to this headline")

Security Onion is configured to support a maximum number of `14000` Wazuh agents reporting to a single Wazuh manager.

# [Automated Deployment](https://docs.securityonion.net/en/2.3/wazuh.html#automated-deployment "Permalink to this headline")

If you would like to automate the deployment of Wazuh agents, the Wazuh server includes `ossec-authd`. You can read more about `ossec-authd` at [https://documentation.wazuh.com/3.13/user-manual/reference/daemons/ossec-authd.html](https://documentation.wazuh.com/3.13/user-manual/reference/daemons/ossec-authd.html).

When using `ossec-authd`, be sure to add a firewall exception for agents to access port `1515/tcp` on the Wazuh manager node by running [so-allow](https://docs.securityonion.net/en/2.3/so-allow.html#so-allow) and choosing the `r` option.

# [API](https://docs.securityonion.net/en/2.3/wazuh.html#api "Permalink to this headline")

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

# [Diagnostic Logging](https://docs.securityonion.net/en/2.3/wazuh.html#diagnostic-logging "Permalink to this headline")

Wazuh diagnostic logs are in `/nsm/wazuh/logs/`. Depending on what you’re looking for, you may also need to look at the [Docker](https://docs.securityonion.net/en/2.3/docker.html#docker) logs for the container:

sudo docker logs so-wazuh

# [More Information](https://docs.securityonion.net/en/2.3/wazuh.html#more-information "Permalink to this headline")

See also

For more information about Wazuh, please see [https://documentation.wazuh.com/3.13/](https://documentation.wazuh.com/3.13/).

# Installing Wazuh With Security Onion

As detailed in my previous [post](https://noctedefensor.com/security-onion-with-hyper-v/),  Security Onion provides a very capable network monitoring solution. It’s capability can be enhanced by installing Wazuh with the Security Onion. Wazuh is a Host intrusion detection and prevention system.  It can be installed as a very capable stand-alone product or in this case integrated with Security Onion. The Wazuh version that is currently integrated with Security Onion is 3.13.1. Due to this integration, the procedures for configuring Wazuh and its capabilities are slightly different then what would be expected with a stand-alone installation.  This post will provide detailed instructions with screenshots of how to install and configure a Wazuh Agent as well as Microsoft Sysmon in conjunction with the Security Onion. This will provide excellent host intrusion detection capabilities as well as basic host intrusion prevention capabilities.  I will be using the SwiftonSecurity Default Sysmon Configuration template. 

## Resources

- [Swift On Security Sysmon Template](https://github.com/SwiftOnSecurity/sysmon-config)
- [Microsoft Sysinternals Sysmon Download](https://docs.microsoft.com/en-us/sysinternals/downloads/sysmon)

- [Security Onion Wazuh Documentation](https://docs.securityonion.net/en/2.3/wazuh.html)
- [Wazuh Documentation](https://documentation.wazuh.com/3.13/user-manual/registering/command-line-registration.html)

## Wazuh Agent Installation Instructions

### 1. Prepare the Environment

Security Onion includes a firewall that locks down all traffic by default. Prior to installing the Wazuh agent, We need to run **`so-allow`** to enable agent traffic from the host we intend to install the agent on to reach the Wazuh Manager.  You will need to allow `Wazuh registration service port 1515/tcp` and `Wazuh agent port 1514/tcp`.  Run this command from the Security Onion command line.

[![This shows the menu of the so-allow commandhe](https://noctedefensor.com/wp-content/uploads/2021/07/So-Allow-1024x369.png)](https://noctedefensor.com/wp-content/uploads/2021/07/So-Allow.png)

### 2. Register an Agent with the Wazuh Manager

Because of the so-allow firewall, this command will differ slightly then what’s found in the Wazuh documentation. Refer to the Security Onion Documentation Wazuh Section. The command is `sudo so-wazuh-agent-manage.` You will need to know the IP address from your intended host.  This command is run from the Security Onion command line. Don’t exit the agent-manage menu as we will do other actions next. 

[![Picture of the menu for Wazuh-Agent-manage](https://noctedefensor.com/wp-content/uploads/2021/07/MyNewAgent-1024x519.png)](https://noctedefensor.com/wp-content/uploads/2021/07/MyNewAgent.png)

### 3. Extract Agent Key from Manager

While we are in the `so-wazuh-agent-manage` menu we extract  the key for the agent we just added. We will need this later to input into the agent configuration on the host.  Select “E” and then enter the ID of the agent you just added. Once the key is outputted on the terminal, highlight and copy it. Paste that key into a note document for later use. 

[![](https://noctedefensor.com/wp-content/uploads/2021/07/Extracted-Key-1-1024x321.png)](https://noctedefensor.com/wp-content/uploads/2021/07/Extracted-Key-1.png)

### 4. Download the Agent MSI from the SOC console

Security Onion packages the Wazuh Agent and provides it for download from the SOC menu. To access this menu, you will need to use `so-allow` to allow your target host access to the SOC page via the browser. Alternatively, you can download the MSI via the analyst machine you gave firewall access to in initial Security Onion setup. Then, you can transfer the agent msi file to the intended host via other means such as USB, email, or network share. 

[![](https://noctedefensor.com/wp-content/uploads/2021/07/Agent-Downloads-1024x443.jpg)](https://noctedefensor.com/wp-content/uploads/2021/07/Agent-Downloads.jpg)

### 5. Run the Wazuh Agent MSI

Once you’ve transferred the MSI to your target machine, run it by right clicking it and selecting “run as administrator.” The Wazuh Installation Wizard will then run. Ensure that you select the option to run the configuration utility after installation.  In the Agent-Manage configuration utility GUI you will enter the Security Onion IP address AND the key you extracted and copied into the note document earlier. The “manage” drop down menu contains the options “start, stop, restart and status” After you’ve entered the Security Onion IP and the agent key, select “start” from the “manage” drop down menu.   If you’d ever need to return to the agent-manager utility you can open it by running C:\Program files (x86)\ossec-agent\win32ui.exe

[![](https://noctedefensor.com/wp-content/uploads/2021/07/WazuhAgentManager-1.png)](https://noctedefensor.com/wp-content/uploads/2021/07/WazuhAgentManager-1.png)

## Sysmon Installation and Wazuh Integration

Ok, your Wazuh agent is installed and should be in communication with the manager. It is now gathering, shipping, and analyzing standard Windows Event logs. Its also performing file integrity monitoring, Compliance/vulnerability scanning, intrusion detection, and basic intrusion prevention actions.  If we go one step further and install Sysmon and configure Wazuh to gather and analyze Sysmon logs, we will be greatly increasing our log analysis capabilities. 

### 1.Download Sysmon and SwiftonSecurity Sysmon Template

- Download and extract Sysmon from Microsoft.
- Download [sysmonconfig-export.xml](https://github.com/SwiftOnSecurity/sysmon-config/blob/master/sysmonconfig-export.xml "sysmonconfig-export.xml") from the SwiftonSecurity Github
- Place both the [sysmonconfig-export.xml](https://github.com/SwiftOnSecurity/sysmon-config/blob/master/sysmonconfig-export.xml "sysmonconfig-export.xml") and the sysmon.exe in the same folder. 

[![](https://noctedefensor.com/wp-content/uploads/2021/07/SysmonInFolder-1024x124.png)](https://noctedefensor.com/wp-content/uploads/2021/07/SysmonInFolder.png)

### Install Sysmon Via CMD line

- Open up a CMD prompt with Administrator privileges. (search for “cmd” in taskbar then right click and select “run as administrator”)
- Navigate to the folder where Sysmon.exe and the template is. 
- Run this command 
    
    ```
    sysmon.exe -accepteula -i sysmonconfig-export.xml
    ```
    

### Add Sysmon to Wazuh configuration file

- Open File Explorer and navigate to C:\Program Files (x86)\ossec-agent\
- Find the file “ossec.conf” 
- Right-click and select copy.
- Go to Desktop and select Paste. 
-  Open up the file and add the following:
- <localfile>  
    <location>Microsoft-Windows-Sysmon/Operational</location>  
    <log_format>eventchannel</log_format>  
    </localfile>
- Save the file and close.
- Right click the file and select “copy”
- Navigate back to C:\Program Files (x86)\ossec-agent 
- Right click and paste the new ossec.conf file. UAC will pop up. Elevate as necessary. 

[![](https://noctedefensor.com/wp-content/uploads/2021/07/Sysmon-1024x542.png)](https://noctedefensor.com/wp-content/uploads/2021/07/Sysmon.png)

## Restart Wazuh Agent

- While in the ossec-agent folder, select win32ui.exe and double click to run it. 
- Select “restart” from the “manage” drop down menu.
- Wazuh will now gather and analyze Sysmon logs.
- Open up Security Onion SOC Alert page and/or Kibana to view the new entries. They will show as “Sysmon”, “Ossec”, or “windows_eventlog”

[![](https://noctedefensor.com/wp-content/uploads/2021/07/SeconionOssec-1024x197.png)](https://noctedefensor.com/wp-content/uploads/2021/07/SeconionOssec.png)

[![](https://noctedefensor.com/wp-content/uploads/2021/07/Kibana-1-1024x561.png)](https://noctedefensor.com/wp-content/uploads/2021/07/Kibana-1.png)