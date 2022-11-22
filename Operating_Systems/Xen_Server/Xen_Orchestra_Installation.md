# [How to Install Xen Orchestra](https://christitus.com/how-to-install-xen-orchestra/ "See on original website")

[![✇](https://freshrss.commsnet.org/f.php?b5decd60)Virtualization on Chris Titus Tech | Tech Content Creator](https://freshrss.commsnet.org/i/?get=f_9 "Filter") 

October 11th 2018 at 1:14 pm

This article shows you how to install Xen Orchestra and use the interface via the web. XOA is a very powerful addition to XenServer as it brings auto-updates and making it easier to manage entire farms.

## Step-by-Step Guide

1.  Login to your XenServer via PuTTy OR through XenCenter console
2.  Type `bash -c "$(curl -s http://xoa.io/deploy)"`
    -   Enter a new IP where you will access XOA from web
    -   Wait for this to finish you will see XOA VM popup
    -   _Note: This may take a couple hours if XOA servers are downloading slow. I highly recommend downloading offline installer if installing on multiple servers._
3.  Once Complete, PuTTy to XOA VM (IP Address from prior install)
    -   login with username/password: xoa
    -   set new password for console
    -   register xoa from username and password that you used on their website [https://xen-orchestra.com/](https://xen-orchestra.com/)
        -   `sudo xoa-updater --register`
    -   now run the updater to check for any upgrades
        -   `sudo xoa-updater &#8211;upgrade`
    -   reboot if any upgrades were performed
4.  Open Browser, Login to XOA
    -   login username `admin@admin.net` password `admin`
5.  Add your XenServers – If you only have 1 pool, you only need to add your pool master
    -   Remember: if you are using default self-signed certificates on XenServer enable “unauthorized certificates” and then click disconnected
6.  Enjoy

## Video Walkthrough

I hope you enjoyed this walkthrough of how to install Xen Orchestra and if you have any feedback let me know below. In closing, I really like this product and think it is a great addition to the XenServer and a much-needed improvement over what Citrix offers.