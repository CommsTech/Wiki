---
title: MeshCentral Install on Ubuntu
description: 
published: true
date: 2022-07-25T13:15:54.706Z
tags: 
editor: markdown
dateCreated: 2022-05-21T15:28:21.146Z
aliases:
---
# MeshCentral Install on Ubuntu

Full YouTube Walkthrough located @ https://youtu.be/VlRNsZbM6j0


Install Ubuntu Server
![[Ubuntu_server_22.04_Installing_snapshot.png]]

Reboot the system

After reboot its best practice to run ``` sudo apt-get update && sudo apt-get upgrade -y ```

Add the nodejs repository
```
sudo add-apt-repository universe
```

then install nodejs and npm with the following commands
```
sudo apt install nodejs -y
sudo apt install npm -y
```

Allow access to the lower 1024 ports with
sudo setcap cap_net_bind_service=+ep /usr/bin/node

install meshcentral
```
npm install meshcentral
```
start meshcentral 
``` 
./node_modules/meshcentral
```

to get the server to boot automatically every time they server restarts do the following

```
sudo nano /etc/systemd/system/meshcentral.service
```
 copy and paste the following
 ```
[Unit]
Description=MeshCentral Server

[Service]
Type=simple
LimitNOFILE=1000000
ExecStart=/usr/bin/node /home/user/node_modules/meshcentral
WorkingDirectory=/home/user
Environment=NODE_ENV=production
User=user
Group=default
Restart=always
# Restart service after 10 seconds if node service crashes
RestartSec=10
# Set port permissions capability
AmbientCapabilities=cap_net_bind_service

[Install]
WantedBy=multi-user.target
```

Youll want to change the following lines for your username and path
```
ExecStart=/usr/bin/node /home/user/node_modules/meshcentral
WorkingDirectory=/home/user

User=user
Group=default

for example

ExecStart=/usr/bin/node /home/commstech/node_modules/meshcentral
WorkingDirectory=/home/commstech/meshcentralWD

User=commstech
Group=commstech
```

then run 

```
sudo systemctl enable meshcentral.service
sudo systemctl start meshcentral.service
```

verify there were no errors with the auto start with

```
sudo systemctl status meshcentral.service
```



[MeshCentral Sample Config.json](https://github.com/Ylianst/MeshCentral/blob/master/sample-config-advanced.json)