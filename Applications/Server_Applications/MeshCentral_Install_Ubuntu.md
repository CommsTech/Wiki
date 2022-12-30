Install Ubuntu Server
![[Pasted image 20221230075550.png]]

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

to get the server to boot automatically everytime they server restarts do the following

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

