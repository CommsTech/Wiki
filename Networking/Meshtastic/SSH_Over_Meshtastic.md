---
title: SSH over Meshtastic
description: 
published: 
tags:
---
# SSH_Over_Meshtastic
You need 2 linux-powered devices and 2 meshtastic-capable devices. You are going to make them communicate with each other through a remote connection over LoRa powered by the meshtastic network.  
_Well, you could still self-connect a computer over SSH. That would require you only 1 device. But if you understand this, you probably already know what’s written in this guide and you’re wasting your time here. Also, it wouldn’t really make much sense. It defeats the purpose of SSH except probably for testing if the SSH daemon is correctly listening to connections on the PC you are working on._

1. install SSH on both the computers you are using for this experiment. One of them needs a SSH server and the other an SSH client. If don’t know how to install/configure that, google it. Some useful links for the most popular distributions:

- [https://ubuntu.com/server/docs/service-openssh 6](https://ubuntu.com/server/docs/service-openssh)
- [OpenSSH :: Fedora Docs 3](https://docs.fedoraproject.org/en-US/fedora/f33/system-administrators-guide/infrastructure-services/OpenSSH/)
- [OpenSSH - ArchWiki 2](https://wiki.archlinux.org/index.php/OpenSSH)

2. Let’s call computer **A** the **server** and computer **B** the **client**. Make sure you configure the ssh daemon accordingly (it’s required to run only on A and check that its `ListenAddress` is `0.0.0.0`, it should be the default configuration on `/etc/ssh/sshd_config` or equivalent filepath, depending on your linux distribution). Connect your meshtastic nodes to A and B via USB.
    
3. Make sure you have the `meshtastic` python cli and the `pytap2` module installed on both A and B. Guide for it [here 50](https://github.com/meshtastic/Meshtastic-python) (it’s in the README, scroll down).
    
4. Open a terminal window and, as a privileged user, run `meshtastic --tunnel` on both A and B. Keep this terminal open for as long as you want to use a connection between A and B. This command will do all the network heavy lifting. It will create a software network interface and assign a IP address to each of your computers. By default the IP subnet that meshtastic creates for you is in the `10.155.0.0/16` range. You are not exactly required to know this, but you need to take note of the IPs that `meshtastic` assign to A and B. In my case it was **`10.115.91.76`** for **A** and **`10.115.208.212`** for **B**, but as far as I’m aware it could be 2 random IPs in the `10.155.0.0/16` address space.
    
5. Once you reached this stage, you just need to get back to the ssh guides and come out with something like `ssh USERNAME@10.115.208.76` from a new terminal of computer B in order to connect to A. If you have issues with the permissions, check whether the SSH `PasswordAuthentication` is allowed or not from the SSH configuration files and/or you copy-pasted the SSH keys correctly if you followed the more secure passwordless authentication route.  
    In order to leverage the greater stability offered by mosh make sure mosh is installed on both the computers. The command then becomes `mosh USERNAME@10.115.208.76` from computer B. The meshtastic software network and the meshtastic node will take care of the magic required for it to work. It takes a while for the connection to establish in the `shortfast` preset and a huge time for the `longslow` preset if you don’t hit some TCP or SSH handshake timeout first. I suggest you to try it out in the `shortfast` mode as it has much more throughput and less latency overall (at the expense of the range of course, no free lunch here).
    

If the command of the last point completed successfully, you should see the shell of your remote computer A appearing on the terminal of computer B. From B you can now run shell commands that A takes care of executing, with all the limitations and/or features that this implies.