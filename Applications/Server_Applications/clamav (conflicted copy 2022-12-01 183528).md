---
title: ClamAV
description: ClamAV antivirus scanning
published: true
date: 2022-07-18T02:41:39.777Z
tags: Antivirus, Scanning
editor: markdown
dateCreated: 2022-07-18T02:41:39.777Z
---
# clamav

> Clam AntiVirus (**clamav**) is a free software, cross-platform antimalware toolkit able to detect many types of malware, including viruses. It was developed for Unix and has third party versions available for AIX, BSD, HP-UX, Linux, macOS, OpenVMS, OSF (Tru64) and Solaris. As of version 0.97.5, ClamAV builds and runs on Microsoft Windows. Both ClamAV and its updates are made available free of charge. One of its main uses is on mail servers as a server-side email virus scanner. 
>
> Sourcefire, developer of intrusion detection products and the owner of Snort, announced on 17 August 2007 that it had acquired the trademarks and copyrights to ClamAV from five key developers.  Upon joining Sourcefire, the ClamAV team joined the Sourcefire Vulnerability Research Team (VRT).  In turn, Cisco acquired Sourcefire in 2013. The Sourcefire VRT became Cisco Talos, and ClamAV development remains there.
>
> [Wikipedia](https://en.wikipedia.org/wiki/Clam%20AntiVirus)


# How to Install and Use ClamAV on Ubuntu

**Clam AntiVirus** (ClamAV) is a free and open source command line interface antivirus software program. It is used to detect trojans and malicious softwares including viruses. It can scan files quickly and can scan over one million viruses and trojans. One of its main uses is to scan emails on mail gateways. **ClamAV** is supported by the following Linux Operating Systems Ubuntu (16.04, 18.04), Debian (7,8), CentOS (6,7). In this blog we will discuss how to install and use **ClamAV** in Ubuntu.

### Does Ubuntu Need AntiVirus?

If you have been using Windows Operating system, you will be familiar with antiviruses. Antiviruses are the software programs which run in the background and check for the viruses which cause problems.

It is most commonly known as viruses do not exist in Linux but it is not true. Malwares and viruses do exist in Linux but they are not quite common. If you want to make your Linux machine more secure then you need to install antivirus.

### Installing ClamAV

In order to install **ClamAV** on your machine, first of all run the following command to update your system

ubuntu@ubuntu:~$ sudo apt-get update

[![](https://linuxhint.com/wp-content/uploads/2020/03/1-47.png)](https://linuxhint.com/wp-content/uploads/2020/03/1-47.png)

After updating your machine, now run the following command to install **ClamAV**

ubuntu@ubuntu:~$ sudo apt-get install clamav clamav-daemon

[![](https://linuxhint.com/wp-content/uploads/2020/03/2-49.png)](https://linuxhint.com/wp-content/uploads/2020/03/2-49.png)

Now **ClamAV** has been installed on your machine. Run the following command to check whether it has been installed or not

ubuntu@ubuntu:~$ clamscan --version

[![](https://linuxhint.com/wp-content/uploads/2020/03/3-42.png)](https://linuxhint.com/wp-content/uploads/2020/03/3-42.png)

If the above command gives the version of **ClamAV** then it has been installed successfully.

### Updating the ClamAV Signature Database

So far you have installed **ClamAV** on your machine, now you need to update the **ClamAV** signature database. To install **ClamAV** signature database, follow the given steps

-   Stop freshclam service
-   Update the signature database (Two methods)
    -   Update by running the command in the terminal
    -   Update by downloading daily.cvd file
-   Start freshclam service

**First step** is to stop the clamav-freshclam service by running the following command in the terminal window

ubuntu@ubuntu:~$ sudo systemctl stop clamav-freshclam

In the **Second Step**, now we have to update the signature database manually. There are two ways to do so. First Method involves to run the following command in the terminal

ubuntu@ubuntu:~$ sudo freshclam

This command will install the signature database in your machine. If this command does not work, then goto the following link to download signature database file

[https://database.clamav.net/daily.cvd](https://database.clamav.net/daily.cvd)

Now create a directory named “clamav”, if does not exist, in a specific location by running the following command

ubuntu@ubuntu:~$ sudo mkdir /var/lib/clamav

And move the downloaded file in this location by running the following command

ubuntu@ubuntu:~$ cp daily.cvd /var/lib/clamav/daily.cvd

Now the **third step** is to start the clamav-freshclam service by running the following command.

ubuntu@ubuntu:~$ sudo systemctl start clamav-freshclam

[![](https://linuxhint.com/wp-content/uploads/2020/03/4-39.png)](https://linuxhint.com/wp-content/uploads/2020/03/4-39.png)

In the above command, we have used some options. These options have following meanings

-   –infected: prints only infected files
-   –remove: removes infected files
-   –recursive: all the subdirectories in the directory will be scanned

You can use further options with this command. Run the following command in the terminal window to see all the available options

ubuntu@ubuntu:~$ man clamscan

[![](https://linuxhint.com/wp-content/uploads/2020/03/5-38.png)](https://linuxhint.com/wp-content/uploads/2020/03/5-38.png)

You can scan your whole ubuntu system by running the following command in the terminal window

ubuntu@ubuntu:~$ sudo clamscan --infected --recursive --remove /

**NOTE:** This can take some time depending upon the amount of data and processing speed of your system

### Installing ClamTK

**ClamTK** is a Graphical User Interface for **ClamAV** software program. If you have issues while using the ClamAV command line interface, you can install **ClamTK** that is a graphical user interface for **ClamAV**. In order to install **ClamTK**, run the following command in the terminal window

ubuntu@ubuntu:~$ sudo apt-get install clamtk

[![](https://linuxhint.com/wp-content/uploads/2020/03/6-35.png)](https://linuxhint.com/wp-content/uploads/2020/03/6-35.png)

### Getting Started with ClamTK

Here we will discuss how to scan a directory using **ClamTK**. First of all run the following command to start **ClamTK** package

ubuntu@ubuntu:~$ clamtk

A window, as shown in the following figure, will appear

[![](https://linuxhint.com/wp-content/uploads/2020/03/7-36.png)](https://linuxhint.com/wp-content/uploads/2020/03/7-36.png)

Now click on the “Scan a directory” from the analysis group and select the desired directory. **ClamTK** will scan that directory and display the result as shown in the following figure

[![](https://linuxhint.com/wp-content/uploads/2020/03/8-28.png)](https://linuxhint.com/wp-content/uploads/2020/03/8-28.png)

### Performance Comparison between ClamAV and ClamTK

While using **ClamAV** and **ClamTK**, I did not notice any difference between the performance of the two packages. So you can use any of them but the best choice is to use **ClamTK** as it provides a graphical user interface which makes it easier to use, specially for beginners.

### Uninstalling ClamAV and ClamTK

So far we have discussed how to install and use **ClamAV** and **ClamTK**, so what if you want to remove these packages from your system? You can remove these packages by running the following commands

ubuntu@ubuntu:~$ sudo apt-get remove clamav clamav-daemon

### Scanning a Directory

Now **ClamAV** is ready to use and can be used in Ubuntu by using clamscan command. Run the following command to scan the desktop directory.

ubuntu@ubuntu:~$ sudo clamscan --infected --remove --recursive  
/home/ubuntu/Desktop

The above command will scan the Desktop directory and gives us its statistics related to scanning as shown in the figure below

[![](https://linuxhint.com/wp-content/uploads/2020/03/9-26.png)](https://linuxhint.com/wp-content/uploads/2020/03/9-26.png)

When you run the above command, it will automatically remove **ClamTK** as well. Now run the following command to remove unwanted files from your system

ubuntu@ubuntu:~$ sudo apt-get autoremove

[![](https://linuxhint.com/wp-content/uploads/2020/03/10-23.png)](https://linuxhint.com/wp-content/uploads/2020/03/10-23.png)

Now **ClamAV** and **ClamTK** have been removed completely from your system.

### Conclusion

**ClamAV** is an antivirus program which can be used to detect and remove trojans, malicious softwares including viruses from your system. In this blog, we have discussed how to install **ClamAV** in ubuntu. After this we discussed how to update the **ClamAV** signature database and how to use **ClamAV** using the command line interface to scan a directory. After this we discussed how to install **ClamTK** which gives a graphical user interface and makes it easier to use. Then we compared the performance of these packages. At the end we learnet how to remove these two packages completely from your system.

After reading this blog, you will be able to use **ClamAV** and **ClamTK** easily. I have explained everything in detail and hope you will find this blog useful.