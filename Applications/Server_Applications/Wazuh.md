# Wazuh

# [Deploying Wazuh agents on Linux systems](https://documentation.wazuh.com/current/installation-guide/wazuh-agent/wazuh-agent-package-linux.html#deploying-wazuh-agents-on-linux-systems "Permalink to this headline")

The agent runs on the host you want to monitor and communicates with the Wazuh manager, sending data in near real time through an encrypted and authenticated channel.

The deployment of a Wazuh agent on a Linux system uses deployment variables that facilitate the task of installing, registering, and configuring the agent. Alternatively, if you want to download the Wazuh agent package directly, see the [packages list](https://documentation.wazuh.com/current/installation-guide/packages-list.html) section.

Note

 

To execute all the commands, root user privileges are required.

## [Add the Wazuh repository](https://documentation.wazuh.com/current/installation-guide/wazuh-agent/wazuh-agent-package-linux.html#add-the-wazuh-repository "Permalink to this headline")

Add the Wazuh repository to download the official packages.

1.  Install the GPG key:
    
    > # curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | apt-key add -
    
2.  Add the repository:
    
    > # echo "deb https://packages.wazuh.com/4.x/apt/ stable main" | tee -a /etc/apt/sources.list.d/wazuh.list
    
3.  Update the package information:
    
    > # apt-get update
    
## [Deploy a Wazuh agent](https://documentation.wazuh.com/current/installation-guide/wazuh-agent/wazuh-agent-package-linux.html#deploy-a-wazuh-agent "Permalink to this headline")

1.  To deploy the Wazuh agent to your system, select your package manager and edit the `WAZUH_MANAGER` variable to contain your Wazuh manager IP address or hostname.

```
WAZUH_MANAGER="192.168.your.ip" apt-get install wazuh-agent
```

2. Enable and start the Wazuh agent service.

`systemctl daemon-reload`
### systemctl enable wazuh-agent
```
systemctl daemon-reload
systemctl enable wazuh-agent
systemctl start wazuh-agent
```