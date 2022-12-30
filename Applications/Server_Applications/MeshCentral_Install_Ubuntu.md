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
start meshcentral ``` ./node_modules/meshcentral
```
