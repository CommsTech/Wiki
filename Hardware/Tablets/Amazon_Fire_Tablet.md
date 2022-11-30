## How to Speed Up Your Amazon Fire Tablet

[](https://christitus.com/author/)| Jul 21, 2020| [Android](https://christitus.com/categories/android)

This Guide goes over over debloating amazon fire tablets and getting Google Store and regular tablet functionality out of them.

## Tools and Sources Used

-   Debloat Amazon Fire (XDA Developers): [Amazon Fire Toolbox](https://forum.xda-developers.com/hd8-hd10/development/official-amazon-fire-toolbox-v1-0-t3889604)
-   Screenshare Any Android Device (GitHub Project): [scrcpy](https://github.com/Genymobile/scrcpy)
    -   YouTube Video Link for scrcpy: [https://youtu.be/VMQfH2Qkuss](https://youtu.be/VMQfH2Qkuss)

## Prerequisites

-   No Root Required
-   Developer Mode Unlocked
-   USB Debugging Enabled in Developer Tools
-   Compatibility:

```fallback
Amazon Fire 8/8+ (2020)
Amazon Fire 10 (2019)
Amazon Fire 7 (2019)
Amazon Fire 8 (2018)
Amazon Fire 10 (2017)
Amazon Fire 8 (2017)
Amazon Fire 7 (2017)
Amazon Fire HD8 (2016)
Amazon Fire HD10 (2015)
Amazon Fire HD8 (2015)
Amazon Fire HD7 (2015)
Amazon Fire HD7 (2014)
Amazon Fire HD6 (2014)
```

Copy

## Enabling Developer Mode

---

![Dev Tools](https://d33wubrfki0l68.cloudfront.net/f3220816ced72cd0a98bdfc213a172a08bd94f01/a700a/images/2020/debloat-amazon/dev-tools.jpg)

---

-   Settings > Device Options > About Fire Tablet and tap on the Serial Number until you unlock Developer Options
-   Settings > Device Options > Developer Tools and enable USB Debugging

---

After doing this plug your tablet in to your computer and tap authorize device. If you don’t see authorization prompt, change USB mode to file transfer or it may already be authorized from a past attempt. Check the Toolbox app in the next step to verify.

## Amazon Fire Toolbox Usage

### Main Menu

---

![Toolbox](https://d33wubrfki0l68.cloudfront.net/f3e167f0b8e285760ad4f66185f138774a19abbf/76a1a/images/2020/debloat-amazon/toolbox.png)

---

#### Options you need

1.  Manage Everything Amazon
    -   Disable all amazon apps
2.  Custom Launcher
    -   Change to Nova Launcher from Amazon
3.  Google Services (Manage)
    -   Install Google Play and Google Sign-In

#### Optional Features on Main Menu

-   ADB Shell
    -   _Command Line for ADB_
-   Hybrid Apps
    -   _Installing Netflix or Disney- Not needed when using Google Store_
-   Lockscreen Wallpaper
    -   _Doesn’t remove ads - Done with another package_
-   Density Modifier
    -   _Changes DPI_
-   Google Assistant
    -   _Uses Google Assistant instead of Alexa_
-   Modify System Settings
    -   _Disable automatic system/app updates, and turn off Over the Air updates_
-   Power Options
    -   _Power Off, Reboot, Bootloader Selection_

### Second Menu

---

![Toolbox2](https://d33wubrfki0l68.cloudfront.net/9ae12f41b0ba903e3ab35325afbd86a720e0361b/16a97/images/2020/debloat-amazon/toolbox2.jpg)

---

#### Recommended Options

-   Remove Lockscreen Ads
    
    -   This setups an automation sequence that constantly scans and removes amazon ads. Here are the detailed instructions:
-   Click Remove Lockscreen Ads in Amazon Fire Toolbox
    
-   Launch Automate Settings
    
-   Check “Run on system startup”
    
-   Import Adblocker script into Automate
    
    -   To import: Click Import from 3 dots on right, Select SD Card and click “Amazon Lockscreen Ads Remover V4.5” from root
-   Return to Home screen of Automate and click the Ads Remover
    

#### Optional Items on Second Menu

-   Parental Control Hide
-   System Backup
    -   _Excellent backup and restore tool for your tablet_
-   Push and Pull
    -   _File Transfer to and from tablet_
-   User Management
    -   _Add Accounts for Google, LinkedIn, Patreon, etc._
-   YouTube Clients
    -   _APK files for Vanced and other YouTube alternative apps_
-   Screen Capturing
    -   _Record or Screenshot on your tablet_
-   Sideload Apps
    -   _Select APK files to upload OR search for APK files on internet_

## Conclusion

This tool is a fantastic way to debloat a new fire tablet and get greater functionality from it. The added utilities I rarely use, but can be very nice for some users.

[#Amazon Fire](https://christitus.com/tags/amazon-fire)