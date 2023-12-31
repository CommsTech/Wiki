---
title: Nvidia Server Driver Install Tesla K80
description: 
published: 
tags:
---
# How to Install Drivers on Ubuntu for the NVIDIA Tesla K80

Posted on  [December 11, 2022](https://www.zb-c.tech/2022/12/11/how-to-install-drivers-on-ubuntu-for-the-nvidia-tesla-k80/) by [tyler.lindberg](https://www.zb-c.tech/author/tyler-lindberg/)

Greetings!  
  
I recently had to work on installing a Tesla K80 in a SUPERMICRO 1027GR-TRF triple GPU server. Then, I needed to install Ubuntu Server 20.04.5 LTS to run batch based GPU workloads.  
  

![](https://www.zb-c.tech/wp-content/uploads/2022/12/image.png)

SUPERMICRO 1027GR-TR w/ E5-2670, 64GB RAM, and 1x Tesla K80

Lets see… What do we need to do to get this working?

**The GPU Driver**

First, we need to install our GPU drivers. Unforutatly the Linux Driver for the K80, 460.106.00, was last updated on 10-26-2021 and will most likely contain unpatched CVEs. This includes just the GPU Driver, and not the CUDA Dev kit.  

Install the pre-requisites for the GPU driver:  
  
```
sudo apt update -y && sudo apt install make gcc wget -y  
```  
Then, download the driver:  
  
```
cd ~ && wget https://us.download.nvidia.com/tesla/470.223.02/NVIDIA-Linux-x86_64-470.223.02.run
```
Then,
if you, for some reasons need to install the drivers from nvidia's page you should make sure you had blacklisted nouveau drivers first:

```
sudo vim /etc/modprobe.d/blacklist.conf
```

Add the following lines:

```
blacklist amd76x_edac 
blacklist vga16fb
blacklist nouveau
blacklist rivafb
blacklist nvidiafb
blacklist rivatv
```

Lastly, install the driver and follow the wizard:

```
chmod +x NVIDIA-Linux-x86_64-470.223.02.run && sudo ./NVIDIA-Linux-x86_64-470.223.02.run
```

At the end your nvidia-smi output should look like this... but with the newest driver:  

![](https://www.zb-c.tech/wp-content/uploads/2022/12/image-3.png)

  
**The Conclusion**

At this point the system should be ready to go for GPU workloads to be installed. After installing this driver, and the needed packages, I was able to run python PyTorch workloads, and Python AI-Benchmarking software. I’ll be writing a post on sub $100 EBAY GPUs, and their ML Performance soon!