---
title: Nextcloud
description: Nextcloud information
dateCreated: 2022-08-30T04:27:17.152Z
published: true
editor: markdown
tags:
  - Server
  - Media
  - Management
  - Storage
  - Collaberation
dateModified: 
---
# Nextcloud

[Source Website](https://nextcloud.com/)



Setup WEBDAV drive mapping

### Mapping drives with Windows Explorer[](https://docs.nextcloud.com/server/latest/user_manual/en/files/access_webdav.html#mapping-drives-with-windows-explorer "Permalink to this heading")

To map a drive using the Microsoft Windows Explorer:

1.  Open Windows Explorer on your MS Windows computer.
    
2.  Right-click on **Computer** entry and select **Map network drive…** from the drop-down menu.
    
3.  Choose a local network drive to which you want to map Nextcloud.
    
4.  Specify the address to your Nextcloud instance, followed by **/remote.php/dav/files/USERNAME/**.
    

> For example:
> 
> https://nextcloud.commsnet.org/remote.php/dav/files/YOURuserNAME/



### LDAP Integration in Nextcloud

|   |   |
|---|---|
||Configure LDAP Integration to NextCloud to login with LDAP user accounts.|
|[1]|[Install OpenLDAP Server, refer to here](https://www.server-world.info/en/note?os=Ubuntu_20.04&p=openldap&f=1).|
|[2]|Configure NextCloud to access to LDAP Server from PHP scripts.|

|   |
|---|
|root@dlp:~# <br><br>[apt](https://www.server-world.info/en/command/html/apt.html) -y install php-ldap<br><br>root@dlp:~# <br><br>[systemctl](https://www.server-world.info/en/command/html/systemctl.html) restart php7.4-fpm|

|   |   |
|---|---|
|[3]|Login to NextCloud Web with admin account and open [Apps].|

|   |
|---|
|![](https://www.server-world.info/en/Ubuntu_20.04/nextcloud/img/59.png)|

|   |   |
|---|---|
|[4]|Select [Your apps] on the left pane and Click [Enable] button on [LDAP user and group backend] section.|

|   |
|---|
|![](https://www.server-world.info/en/Ubuntu_20.04/nextcloud/img/60.png)|

|   |   |
|---|---|
|[5]|After enabling [LDAP user and group backend], open settings again and select [Administration] - [LDAP / AD integration] on the left pane.|

|   |
|---|
|![](https://www.server-world.info/en/Ubuntu_20.04/nextcloud/img/61.png)|

|   |   |
|---|---|
|[6]|Input LDAP server's information to connect.  <br>Like this example to use OpenLDAP on Ubuntu Server, it's OK to input server's hostname or IP address and Base DN only.  <br>Click [Test Base DN] button and if [Configuration OK] message is displayed like follows, that's OK, Click [Continue] to proceed.<br><br>For [User DN] and its [Password] fields, generally input a LDAP user account to bind LDAP server. However, OpenLDAP on Ubuntu server, anonymous bind is enabled by default as following setting, so it does not need to input them.  <br>⇒ olcAccess: {0}to attrs=userPassword by self write by anonymous auth by * none|

|   |
|---|
|![](https://www.server-world.info/en/Ubuntu_20.04/nextcloud/img/62.png)|

|   |   |
|---|---|
|[7]|Configure on [Users] tab.  <br>It's OK with default setting if using OpenLDAP on Ubuntu Server like this example.  <br>Confirm [Configuration OK] and Click [Continue] to proceed.|

|   |
|---|
|![](https://www.server-world.info/en/Ubuntu_20.04/nextcloud/img/63.png)|

|   |   |
|---|---|
|[8]|Configure on [Login Attributes] tab.  <br>It's OK with default setting if using OpenLDAP on Ubuntu Server like this example.  <br>Confirm [Configuration OK] and Click [Continue] to proceed.|

|   |
|---|
|![](https://www.server-world.info/en/Ubuntu_20.04/nextcloud/img/64.png)|

|   |   |
|---|---|
|[9]|Configure on [Groups] tab.  <br>If you'd like to limit groups they can search LDAP directory, configure here.  <br>But it's OK with default setting if using OpenLDAP on Ubuntu Server like this example.  <br>Confirm [Configuration OK] and finish configuration for admin account.|

|   |
|---|
|![](https://www.server-world.info/en/Ubuntu_20.04/nextcloud/img/65.png)|

|   |   |
|---|---|
|[10]|Move to Login form and specify a LDAP user.|

|   |
|---|
|![](https://www.server-world.info/en/Ubuntu_20.04/nextcloud/img/66.png)|

|   |   |
|---|---|
|[11]|If configuration OK, it's possbile to login to NextCloud with LDAP users like follows.|

### Fixing the Chuck upload issues
### Fix:

 I found simply setting the maximum chunk size to 50 MB (half of Cloudflare's 100 MB upload size limit) worked to resolve this issue.

I put together a short guide to fix this issue with the latest stable release ([3.4.4](https://github.com/nextcloud/desktop/releases/tag/v3.4.4), but should work on any client v3.4+). I tried to make it as accessible as possible to follow.

### Windows Fix

Press `Win+R` on your keyboard to open the `Run` application. Past the following in the dialog box:

`%APPDATA%\Nextcloud\nextcloud.cfg`

This will either ask you to pick an application to open `nextcloud.cfg` or will open in your default text editor (unless you have something else set to open .cfg files). If it asks you to pick an application, feel free to use Notepad or any other editor.

Add the following line under the `[General]` section:

`maxChunkSize=50000000`

Save the file, quit Nextcloud desktop, and start it again.

### MacOS Fix

Open a Finder window and press `Command+Shift+G` on your keyboard. This will bring up a 'Go to folder' window. Paste the following in the dialog box:

`$HOME/Library/Preferences/Nextcloud`

Open the `nextcloud.cfg` file. If you do not have a default editor for .cfg files, feel free to open the file with TextEdit.

Add the following line under the `[General]` section:

`maxChunkSize=50000000`

Save the file, quit Nextcloud desktop, and start it again.

### Linux Fix

Open a terminal window and edit the following file:

`nano $HOME/.config/Nextcloud/nextcloud.cfg`

Add the following line under the `[General]` section:

`maxChunkSize=50000000`

Save the file (`Ctl+o`, `Ctl+x`), then quit Nextcloud desktop, and start it again.


# NEXTCLOUD + CLAMAV ANTIVIRUS

 Wed, Jul 21, 2021  17-minute read

### Table of Contents

- [Requirements](https://rair.dev/nextcloud-clamav-antivirus/#0-requirements-)
- [Overview](https://rair.dev/nextcloud-clamav-antivirus/#1-overview-)
    - [Docker vs. Bare Metal](https://rair.dev/nextcloud-clamav-antivirus/#2-docker-vs-bare-metal-)
- [Preparation](https://rair.dev/nextcloud-clamav-antivirus/#3-preparation-)
- [Install ClamAV on the Host Machine (Recommended method)](https://rair.dev/nextcloud-clamav-antivirus/#4-install-clamav-on-the-host-machine-recommended-)
    - [Check Virus Database and Daemon Status](https://rair.dev/nextcloud-clamav-antivirus/#5-check-virus-database-and-daemon-status-)
    - [Prepare Nextcloud (Docker)](https://rair.dev/nextcloud-clamav-antivirus/#8-prepare-nextcloud-docker-)
    - [Install Nextcloud ‘Antivirus for files’ App](https://rair.dev/nextcloud-clamav-antivirus/#9-install-nextcloud-antivirus-for-files-app-)
- [Install ClamAV via Docker](https://rair.dev/nextcloud-clamav-antivirus/#11-install-clamav-via-docker-)
    - [Add ClamAV Container to Nextcloud Network](https://rair.dev/nextcloud-clamav-antivirus/#12-add-clamav-container-to-nextcloud-network-)
    - [Install Nextcloud “Antivirus for files” App](https://rair.dev/nextcloud-clamav-antivirus/#13-install-nextcloud-antivirus-for-files-app-)
- [Option for Low-Memory systems](https://rair.dev/nextcloud-clamav-antivirus/#15-option-for-low-memory-systems-)
    - [Install ClamAV on the Host Machine](https://rair.dev/nextcloud-clamav-antivirus/#16-install-clamav-on-the-host-machine-)
    - [Install Nextcloud “Antivirus for files” App](https://rair.dev/nextcloud-clamav-antivirus/#17-install-nextcloud-antivirus-for-files-app-)
- [Test the System](https://rair.dev/nextcloud-clamav-antivirus/#19-test-the-system-)
    - [Upload the EICAR Test Files](https://rair.dev/nextcloud-clamav-antivirus/#20-upload-the-eicar-test-files-)
    - [Check Background Scans](https://rair.dev/nextcloud-clamav-antivirus/#21-check-background-scans-)
- [Bonus Extras](https://rair.dev/nextcloud-clamav-antivirus/#22-bonus-extras-)
    - [Add Push Notifications](https://rair.dev/nextcloud-clamav-antivirus/#23-add-push-notifications-)
    - [ClamAV Tweaks](https://rair.dev/nextcloud-clamav-antivirus/#24-clamav-tweaks-)
- [What do I do when an infected file is found?](https://rair.dev/nextcloud-clamav-antivirus/#31-what-do-i-do-when-an-infected-file-is-found-)
- [Conclusion](https://rair.dev/nextcloud-clamav-antivirus/#32-conclusion-)

At this point we all have a good understanding of what a computer virus is (or at least what it can do to your computer). Fortunately for us, there’s a great open source tool called ClamAV which integrates well with Nextcloud. This allows for both background scanning of files, and scanning of all new uploads. The [official documentation](https://docs.nextcloud.com/server/latest/admin_manual/configuration_server/antivirus_configuration.html) actually does a pretty good job of covering this topic, but I had a few snags installing ClamAV for Nextcloud and wanted to share them here.

---

[![Buy Me A Coffee](https://rair.dev/images/ko-fi.png)](https://ko-fi.com/A0A74AJX7)

Thank you for visiting my site and checking out this post! I hope you find it helpful. You may have noticed I don’t have any advertisements running (I hate how invasive online advertising has become). This also means no passive income to keep the site running. Please consider donating a small amount to say thank you and help me cover the costs.

---

## Requirements

- A working Nextcloud instance (via Docker or Bare Metal)
- Root access to your server
- Basic familiarity with Linux command line
- **Minimum** 2GB of RAM (not required but highly recommended)

## Overview

The screenshots and commands are from a Ubuntu 20.04 server since it is one of the most commonly deployed, but will work on just about any server. You may notice the requirement of 2GB of RAM. While normally Nextcloud can run on as little as 512MB, the ClamAV scanner loads the virus database into RAM and will occupy **1-1.3GB of RAM**! If you are running on a very low RAM machine, there is an alternative which [I discuss below](https://rair.dev/nextcloud-clamav-antivirus/#15-option-for-low-memory-systems-), but is generally not recommended as it will severely impact performance.

Like many Nextcloud Apps, the one you install via Nextcloud Hub is only a _connector_ and does not include the actual virus scanning software. That we must setup separately.

Once the ClamAV packages and ClamAV for Nextcloud app is installed, we will use the 4 [EICAR test files](https://www.eicar.org/?page_id=3950) to make sure the antivirus software is working and behaving as expected. Note that these files are industry-standard test files that contain no actual viruses in them.

### Docker vs. Bare Metal

If you’ve seen any of my other posts, you will probably note that I prefer to use Docker when running Nextcloud. This is primarily to handle dependencies that can get out of hand when you want to run multiple micro-services on the same server. The good news is, my preferred way to integrate ClamAV is actually identical no matter how you have Nextcloud installed.

I prefer to install ClamAV directly on the host instead of using a Docker container. This allows me to use ClamAV directly with Nextcloud (in a Docker container or not) and also [scan other files on the host](https://aaronbrighton.medium.com/installation-configuration-of-clamav-antivirus-on-ubuntu-18-04-a6416bab3b41). While we can also scan host system files running ClamAV in a Docker container it takes a bit more effort.

I will also show how to run ClamAV in a Docker container if that is the route you wish to pursue.

## Preparation

To give you an idea of how the antivirus scanner works, I have created a new Nextcloud environment (v22) and uploaded one of the EICAR test files. The file uploaded just fine:

![eicar test file uploads fine](https://rair.dev/nextcloud-clamav-antivirus/01-infected-zip.png)

One of the EICAR test files uploaded to my Nextcloud.

## Install ClamAV on the Host Machine (Recommended method)

This first approach, I believe, gives the most flexibility to use the antivirus scanner within Docker and outside if you so wish. Let’s begin by grabbing the 3 required packages:

```bash
$ sudo apt update
$ sudo apt install clamav clamav-daemon clamav-freshclam
```

> The above will install the 3 key pieces of the ClamAV antivirus software.
> 
> - **clamav** – The base of the program that can be used to scan a file or directory when you want. Use the command `clamscan`.
> - **clamav-daemon** – The `systemd` unit that runs ClamAV in the background. This is what allows Nextcloud to run frequent filesystem scans as well as scanning files when they are uploaded. To use otherwise, use the command `clamdscan`.
> - **clamav-freshclam** – The `systemd` unit that runs the virus definition update in the background. By default it will check for updates every 30 minutes.

### Check Virus Database and Daemon Status

Before we jump straight in, we should make sure that the virus database has loaded correctly and that the `systemd` unit has started correctly.

#### Virus Database Status

Once installed, be patient as it will likely take a few minutes for the initial virus database to download. You can have a look at the progress with:

```bash
$ sudo cat /var/log/clamav/freshclam.log
```

![check freshclam log](https://rair.dev/nextcloud-clamav-antivirus/02-clamav-logfile.png)

We see `freshclam` updating our virus definitions.

#### Background Virus Scanner Service

Let’s see if the `clamav-daemon` started by using:

```bash
$ sudo systemctl status clamav-daemon.service
```

![clamav daemon failed to start](https://rair.dev/nextcloud-clamav-antivirus/03-failed-systemd-clamav.png)

Oops! Failed to start properly.

Uh-oh, looks like mine didn’t start yet! This is likely due to the service trying to start before the `clamav-freshclam` service had a chance to finish downloading the virus database, resulting in the failure. No worries, let’s restart it.

```bash
$ sudo systemctl restart clamav-daemon.service
```

![clamav daemon successful start](https://rair.dev/nextcloud-clamav-antivirus/04-working-systemd-unit.png)

ClamAV daemon is up and running!

I got no output from the above command, but checking the status again gives me a green light this time. Note that if you check a system resource monitor like `htop`, your memory usage will have increased by ~1GB! Again this is expected as it loads the virus database into memory to speed up scans.

![clamav using 1gb memory](https://rair.dev/nextcloud-clamav-antivirus/05-htop-clamav.png)

Over half the machine’s memory used (for a good cause)!

### Prepare Nextcloud (Docker)

If you have installed Nextcloud via Docker, read on. Otherwise, [skip to the section below.](https://rair.dev/nextcloud-clamav-antivirus/#9-install-nextcloud-antivirus-for-files-app-)

To make ClamAV available to Nextcloud, we must mount the ClamAV ‘socket’ into the Nextcloud container. This will be a volume mount that is declared when the container is started.

Docker-compose

Modify your docker-compose file to include the following in your `volumes:` declaration section:

```yaml
version: 'X.X'

  services:
    nextcloud:
    ...
      volumes:
        - /run/clamav/clamd.ctl:/run/clamav/clamd.ctl
    ...
```

> **Note:**  
> This will mount the socket from the host, directly into the same location in the container.

Docker Run

### Install Nextcloud ‘Antivirus for files’ App

As a Nextcloud admin, head to your Apps hub and click the magnifying glass in the top corner. Search for “Antivirus” and you should see the correct App show up. As of writing this, it is at version 3.2.1:

![install antivirus app within nextcloud](https://rair.dev/nextcloud-clamav-antivirus/06-Nextcloud-antivirus-for-files-app.png)

Nextcloud Antivirus connector App.

Click ‘**Download and enable**‘, followed by your admin password.

#### Configure Antivirus App for Daemon (Socket) Mode

Head to your ‘**Settings**‘ panel by clicking your profile picture in the top right corner. Down in the ‘**Administration**‘ section, you will find a ‘**Security**‘ category.

Scroll down to find the ‘**Antivirus for Files**‘ heading.

![clamav set to Daemon (Socket)](https://rair.dev/nextcloud-clamav-antivirus/08-clamav-daemon-socket.png)

Make sure you are looking at the Administration –> Security.

Complete the following:

- We want to change the ‘**Mode**‘ to ‘**ClamAV Daemon (Socket)**‘.
- The ‘**Socket path**‘ should be automatically populated, but if not, make sure it points to `/var/run/clamav/clamd.ctl` or `/run/clamav/clamd.ctl`.
- The default ‘**Stream Length**‘ setting is of **26214400** bytes is 25MiB, and represents the max file size that will be scanned during uploads. Setting this higher will scan larger uploaded files in real time, but a second modification needs to be made if you want this to be higher ([see below](https://rair.dev/nextcloud-clamav-antivirus/#27-scanning-larger-files-)).
- The ‘**File size limit for periodic background scans**‘ will take care of larger files as long as it is set to **-1** (no limit). Even set at -1, I still had a few errors. [See below for the fix](https://rair.dev/nextcloud-clamav-antivirus/#27-scanning-larger-files-).
- The final setting, ‘**When infected files are found during a background scan**‘ asks what to do when an infected file is found. For now I would leave it set to ‘**Only log**‘ as there are occasional false positives. Infected files will show in the `nextcloud.log` and in your ‘**Activities**‘ app. However, if you’d like a push notification sent, you must [enable it (see below)](https://rair.dev/nextcloud-clamav-antivirus/#23-add-push-notifications-).

There is also an advanced section, but there really shouldn’t be anything to change there.

Move on to the [Test the System](https://rair.dev/nextcloud-clamav-antivirus/#19-test-the-system-) section.

## Install ClamAV via Docker

Some may prefer this method, but it limits the availability of ClamAV to containers it shares a network with. It also makes it difficult to run ClamAV on the host machine (but not impossible). This was the way I started and it worked great, but I prefer the above option. [More info from ClamAV.](https://docs.clamav.net/manual/Installing/Docker.html)

### Add ClamAV Container to Nextcloud Network

Docker-compose

This guide will assume you already have a Nextcloud stack running, and simply wish to add ClamAV to it.:

```yaml
version: 'X.X'

services:
  nextcloud:
  ...
  # Add the official ClamAV container
  clamav:
    image: clamav/clamav:unstable
    # Specify container name for easier mapping later
    container_name: clamav
    # Map the virus database file so it doesn't have to re-download each time
    volumes:
      - /opt/volumes/clamav:/var/lib/clamav
```

> **Note:**  
> This could also be accomplished by creating a `clamav-nextcloud` network, and attaching both ClamAV and Nextcloud containers to said network.

After modifying your `docker-compose` file, make sure to start this container with:

```bash
$ sudo docker-compose up -d
```

Docker Run

Once the container is up and running, it might need a minute or two to fully finish starting up.

### Install Nextcloud “Antivirus for files” App

[Follow the instructions above](https://rair.dev/nextcloud-clamav-antivirus/#9-install-nextcloud-antivirus-for-files-app-), return here once installed.

#### Configure Antivirus App for Daemon Mode

Head to your ‘**Settings**‘ panel by clicking your profile picture in the top right corner. Down in the ‘**Administration**‘ section, you will find a ‘**Security**‘ section.

Scroll down to find the ‘**Antivirus for Files**‘ heading.

![connecting to our dockerized clamav](https://rair.dev/nextcloud-clamav-antivirus/09-clamav-daemon-nosocket.png)

Note the host is the same as the `container_name` we used above.

Complete the following:

- We want to change the ‘**Mode**‘ to ‘**ClamAV Daemon**‘.
- The ‘**Host**‘ should be set to the name of your ClamAV container. In this guide, I set the container name explicitly to `clamav`.
- The ‘**Port**‘ of the container is by default listening on **3310**.
- The default ‘**Stream Length**‘ setting is of **26214400** bytes is 25MiB, and represents the max file size that will be scanned during uploads. Setting this higher will scan larger uploaded files in real time, but a second modification needs to be made if you want this to be higher ([see below](https://rair.dev/nextcloud-clamav-antivirus/#27-scanning-larger-files-)).
- The ‘**File size limit for periodic background scans**‘ will take care of larger files as long as it is set to **-1** (no limit). Even set at -1, Nextcloud gave some errors. [See the fix below](https://rair.dev/nextcloud-clamav-antivirus/#27-scanning-larger-files-).
- The final setting, ‘**When infected files are found during a background scan**‘ asks what to do when an infected file is found. For now I would leave it set to ‘**Only log**‘ as there are occasional false positives. Infected files will show in the Nextcloud.log and in your ‘**Activities**‘ app. However, if you’d like a push notification sent, you must enable it ([see below](https://rair.dev/nextcloud-clamav-antivirus/#23-add-push-notifications-)).

There is also an advanced section, but there really shouldn’t be anything to change there.

Move on to the [Test the System](https://rair.dev/nextcloud-clamav-antivirus/#19-test-the-system-) section below.

## Option for Low-Memory systems

This option will not ultimately lower the requirements for ClamAV. Instead of having the constant overhead of the 1+GB virus definition library loaded into memory, it will only load the definitions on demand. This might seem like a benefit to most, but uploading files and background scans are slow and still require memory.

> **Warning:**  
> This won’t work for a Dockerized Nextcloud instance. To use the ClamAV scanning program (binary) `clamscan`, we also need access to the libraries which it depends on. Mounting all of them into your Nextcloud container is not advised/feasible.

### Install ClamAV on the Host Machine

[Follow the instructions above](https://rair.dev/nextcloud-clamav-antivirus/#4-install-clamav-on-the-host-machine-recommended-) to install the required ClamAV packages on your host machine. The ClamAV `systemd` unit does not need to be started.

### Install Nextcloud “Antivirus for files” App

[Follow the instructions above](https://rair.dev/nextcloud-clamav-antivirus/#9-install-nextcloud-antivirus-for-files-app-), return here once installed.

#### Configure Antivirus App for Executable Mode

Head to your ‘**Settings**‘ panel by clicking your profile picture in the top right corner. Down in the ‘**Administration**‘ section, you will find a ‘**Security**‘ section.

Scroll down to find the ‘**Antivirus for Files**‘ heading.

![clamav executable settings](https://rair.dev/nextcloud-clamav-antivirus/07-security-category-1.png)

The default settings should suffice..

Complete the following:

- We want to ensure the ‘**Mode**‘ is set to ‘**ClamAV Executable**‘.
- The default ‘**Stream Length**‘ setting is of **26214400** bytes is 25MiB, and represents the max file size that will be scanned during uploads. Setting this higher will scan larger uploaded files in real time, but a second modification needs to be made if you want this to be higher ([see below](https://rair.dev/nextcloud-clamav-antivirus/#27-scanning-larger-files-)).
- ‘**Path to clamscan**‘ points to the location of the `clamscan` program (binary). This can be found by running the command `which clamscan`.
- **‘Extra command line options (comma-separated)**‘ allows you to modify the way `clamscan` runs. There are many options, [refer to the documentation](https://docs.clamav.net/manual/Usage/Scanning.html#one-time-scanning).
- The ‘**File size limit for periodic background scans**‘ will take care of larger files as long as it is set to **-1** (no limit). Even set to -1, Nextcloud gave me some errors. [See below for the fix](https://rair.dev/nextcloud-clamav-antivirus/#27-scanning-larger-files-).
- The final setting, ‘**When infected files are found during a background scan**‘ asks what to do when an infected file is found. For now I would leave it set to ‘**Only log**‘ as there are occasional false positives. Infected files will show in the Nextcloud.log and in your ‘**Activities**‘ app. However, if you’d like a push notification sent, you must enable it ([see below](https://rair.dev/nextcloud-clamav-antivirus/#23-add-push-notifications-)).

There is also an advanced section, but there really shouldn’t be anything to change there.

## Test the System

As always, it’s best to test the system to ensure everything is working and any bad files are detected. If you recall from the introduction, I uploaded an EICAR test file (**infected.zip**) to my Nextcloud. This should be picked up by the background scanning service.

As far as I can tell, it is run in conjunction with `cron` jobs. So make sure you have [cron setup and working correctly](https://rair.dev/nextcloud-cron/). My test instance was initially using **AJAX** and did not run the background scan to detect the file. It appears that on larger installations, the background scanner picks a handful of files to scan at a time and eventually works its way through your entire Nextcloud file system.

### Upload the EICAR Test Files

While waiting for background scans, I decided to try and upload the 4 test files from EICAR (see link in [Overview](https://rair.dev/nextcloud-clamav-antivirus/#1-overview-) section) to my Nextcloud. I was greeted with a warning message in the top right of the screen, and lots of error messages in the Nextcloud Log.

![ClamAV successful block test files](https://rair.dev/nextcloud-clamav-antivirus/10-EICAR-upload-denied.png)

All files were blocked from uploading to Nextcloud.

![Nextcloud logged antivirus errors](https://rair.dev/nextcloud-clamav-antivirus/11-EICAR-test-logging.png)

Antivirus hits are easy to spot in the logs.

This looks good, the scanner is effectively checking files as I upload them and ensuring they don’t even make it to Nextcloud.

### Check Background Scans

The background scan eventually ran and caught the **infected.zip** file. I was given a notification in the ‘**Activities**‘ App. Looking in my ‘**Activities**‘ App, the ‘**Antivirus**‘ section at the bottom filters the other activities out.

![background scan detects test file](https://rair.dev/nextcloud-clamav-antivirus/12-background-scan-detected.png)

**Activities** is another easy way to see the status of the Antivirus scanner.

Two things to note:

1. The virus was detected and logged, but the file was **not deleted**. I like to keep it this way, so I can see what is infected and decide what to do with it. Part of the reason is because occasionally there are **false positives**:
2. While testing the size limits of the scanner, I uploaded a large RAW image I was working on. For some reason, the scanner decided it also had the same EICAR test virus in it as well. This is definitely not the case, and upon deleting and uploading again, the file is fine, no false detection.

## Bonus Extras

The above should get you started and provide a good amount of protection against viruses and potential threats. But I wanted to share a few tips I learned from using ClamAV with Nextcloud.

### Add Push Notifications

By default, there are no push notifications. Head to your ‘**Settings**‘ –> ‘**Administration**‘ –> ‘**Activity**’, and scroll to the bottom under ‘**Other Activities**‘. You will see a checkbox for push notifications under ‘**Antivirus detected a virus**‘. Here you can also optionally set email notifications if you have an email service setup.

![enable push notifications for antivirus](https://rair.dev/nextcloud-clamav-antivirus/13-push-notifications-antivirus.png)

Enable push notifications to see threats immediately.

### ClamAV Tweaks

The below settings primarily live in your `/etc/clamav/clamd.conf` file. Below are a few of my preferred settings, but you can also have a look through all of the options available by using the `man clamd.conf` command.

Open with your favorite editor and change the following:

#### Change Socket Permissions

For some reason the default is a world read-write accessible socket. Definitely not a good idea. Let’s change that to remove the permissions for ‘other’ users.

```cfg
LocalSocketMode 660
```

#### Alert When File Too Large

When background scanning, you can use this setting to alert you if a file was not scanned. By default, we set our ‘**Stream Length**‘ to scan files up to 25MiB in size, and no limit on the background scanner. However many files larger than 25MiB were still skipped.

```cfg
AlertExceedsMax true
```

> **Warning:**  
> This will mark files as detected infections, with the `Heuristics.Exceeds.Limit` infection. This is not an infection at all, just warning you that it was not scanned fully. If you see this error constantly, see the tweak below. Once you’ve increased the file scan size, this can safely be set to `false`.

#### Scanning Larger Files

I would occasionally get an error in my Nextcloud logs that there was a problem with ‘Stream Max Size’. Digging further, I realized that setting things like `MaxFileSize`, `MaxScanSize`, etc. did not affect it. This is because of how Nextcloud utilizes the ClamAV daemon by ‘streaming’ a file to it. We instead need to change the `StreamMaxLength`. See above to know how big to change it to.

```cfg
StreamMaxLength 200M
```

I also upped the ‘**Stream Length**‘ setting in my Antivirus App to a larger size (104857600 bytes or 100 MiB).

#### Detect Potentially Unwanted Applications (PUA)

[See the documentation](https://docs.clamav.net/faq/faq-pua.html) on this as it appears to be a big topic. I’ve been using it for a while now with not many false positives. The short version is that it is a new library to detect bad programs across a multitude of platforms (Android, Windows, etc.)

```cfg
DetectPUA true
```

#### Log File Max Size

Log files can be super valuable, but can also grow to massive size and take up valuable space on your drives. Best to limit them so they are rotated regularly.

```cfg
LogFileMaxSize 10M
```

#### Restart the ClamAV Daemon

After making any of the changes above, we will need to restart the `systemd` unit.

ClamAV on Host

```bash
$ sudo systemctl restart clamav-daemon.service
```

ClamAV on Docker

## What do I do when an infected file is found?

![false positives depend on settings](https://rair.dev/nextcloud-clamav-antivirus/14-false-positives.png)

With many settings enabled, I received lots of false positives.

How do you know the file is really infected? How do we know it isn’t a false-positive?

[The answer is difficult](https://www.mail-archive.com/clamav-users@lists.clamav.net/msg50379.html). The internet is your friend. You can search for the infection that was found and usually there is an explanation for what was identified. After that, you will have to use your best judgment.

Ask yourself: Where did this file come from? Do I trust the source? How critical is the file? if I were to delete it, can I find another copy elsewhere?

When in doubt, [make a backup](https://rair.dev/nextcloud-backup-pt-1/) and delete the file, or scan with another virus scanning software.

## Conclusion

Viruses and malware are most commonly distributed via emails and unsolicited links. But occasionally we may receive a file from a friend who unknowingly is passing around an infected file (insert your favorite COVID reference here). Integrating it into Nextcloud is relatively painless and if you have a bit of RAM to spare, can be useful for some added protection.

The ClamAV program is not a full fledged antivirus suite, but does a great job in detecting many threats to our digital lives. It might not be as intuitive as the big names like McAfee, BitDefender, Avira, Malwarebytes, etc. But at least it won’t be pushing you to upgrade to ‘Premium’ constantly.


## Troubleshooting
There are some warnings regarding your setup.

- The PHP module "imagick" is not enabled although the theming app is. For favicon generation to work correctly, you need to install and enable this module.
***does not work
```
sudo apt-get install php-imagick
sudo snap restart nextcloud
```

*** temp solution is just disable theming app in nextcloud