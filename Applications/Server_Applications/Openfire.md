---
title:
description:
published:
aliases: 
tags: 
---
Openfire is a powerful instant messaging (IM) and chat server that implements the XMPP protocol. This document will guide you through installing Openfire. For a full list of features and more information, please visit the Openfire website: [http://www.igniterealtime.org/projects/openfire/](http://www.igniterealtime.org/projects/openfire/)

## Installation

### Windows

Select Openfire installer that is better suiting you (with or without Java JRE, x86 or x64). Run the installer. The application will be installed to C:\Program Files\Openfire by default.

**Note:** On Windows systems we suggest using a service to run Openfire (read the Windows Service section below). When using Openfire Launcher on Windows Vista or newer with UAC protection enabled, it has to be run with Run as administrator option, to be able to write changes to config and embedded database (if used) stored in C:\Program files\Openfire\ folder. If Openfire is running via launcher without Run as administrator option from Program files, it can't get proper permissions to write changes. It shows errors (in red) when running the launcher and during the setup will require the current password for the administrator account (although this is a new installation and normally it doesn't ask for it). This is an effect of missing permissions and Openfire not being able to initialize the database and other resources.

**Since 4.1.5 Openfire installs and runs the service automatically (also opens the browser and loads the web setup page). The launcher (if you want to use it) is also made to run in elevated mode, you don't need to run it as administrator manually. But you shouldn't use the launcher, if the service is running. Because this will create a conflict.**

## pade plugin
# Pade Readme

This project provides a web-based unified communication solution for Openfire.

- peer to peer based chat,
- persistent groupchat,
- audio, video conferencing and live streaming,
- telephone access to conferences,

It includes third-party products, notably:

- [Jitsi Videobridge](https://github.com/jitsi/jitsi-videobridge) project;
- [Jitsi Conference Focus (jicofo)](https://github.com/jitsi/jicofo) project;
- [Jitsi Meet](https://github.com/jitsi/jitsi-meet) web client.
- [Jitsi SIP Gateway](https://github.com/jitsi/jigasi) project.
- [Pàdé](https://github.com/igniterealtime/pade) web desktop client based on the [ConverseJS](https://github.com/conversejs/converse.js) project.
- [ffmpeg](https://www.ffmpeg.org/) live streaming to rtmp server like you-tube.

Pade works with Firefox. It however works best with Chromium based apps like Chrome, Edge, Electron and Opera.

Pade has minimal network requirements and works out of the box internally on a local area network (LAN) or with a hosted Openfire server on the internet. If your Openfire server is placed behind a NAT and firewall and you want to allow external internet access, then you require some network expertise to configure it. You would need to open a few UDP/TCP ports and provide both the public and private IP addresses of your openfire server.

Pade uses an XMPP user called **jvb** that will join a global conference called **ofmeet** with the focus user called **focus**. If you enable the SIP gateway, a new user called **jigasi** will be created and it will join a global conference called **jigasi** with the focus user **focus**

[![image](https://user-images.githubusercontent.com/110731/99916724-af0dc880-2d03-11eb-80c3-b35b9009910a.png)](https://user-images.githubusercontent.com/110731/99916724-af0dc880-2d03-11eb-80c3-b35b9009910a.png)

Pade will not work out of the box if your Openfire server is configured to use LDAP. You would need to create the jvb, focus and jigasi bot users manually. Give the focus bot user owner/admin permissions to the MUC service.

## [](https://www.igniterealtime.org/projects/openfire/plugins/1.7.6/pade/readme.html#installation)Installation

Download latest release from [here](https://github.com/igniterealtime/openfire-pade-plugin/releases) and upload the pade.jar from the admin web console of Openfire. Wait for the plugin to appear in the plugins listing and then complete the following steps to confirm it is working.

Make sure this user is online and has joined the **ofmeet** chat room. Confirm focus user is also online and has joined the **ofmeet** room as well.

[![image](https://user-images.githubusercontent.com/110731/99916763-eb412900-2d03-11eb-9028-c391713d4384.png)](https://user-images.githubusercontent.com/110731/99916763-eb412900-2d03-11eb-9028-c391713d4384.png)

if you have configured a SIP account for jigasi, also confirm that the jigasi user has logged in.

If you have an active focus user, then you can do a quick peer-to-peer test with two browser tabs on your desktop. Open both of them to the same conference like https://your_server:7443/ofmeet/testconf and confirm that it is showing in the conference summary.

If you get audio and video, then focus bot user is working ok and XMPP messages are passing around ok. If not, it is back to the log files and help from the community.

To confirm the video-bridge is working, you need to run the last step again with 3 users. If audio and video stops with third participant, then double check on the network configuration, making sure TCP port 7443 and UDP port 10000 are opened for listening from the openfire server. Otherwise, check the log files and ask for help from ignte-realtime community.

The new summary admin page shows call statistics from JVB2 as well as all active calls. It also shows the status of all enabled Jitsi Components. If you see red crosses instead of green tick icons, then please check you log file for errors. If you **do not do this**, then you will be unable to video conference 3 or more people.

[![image](https://user-images.githubusercontent.com/110731/157767003-3bbef448-5d99-4ae2-afe5-f38e31832b5f.png)](https://user-images.githubusercontent.com/110731/157767003-3bbef448-5d99-4ae2-afe5-f38e31832b5f.png)

## [](https://www.igniterealtime.org/projects/openfire/plugins/1.7.6/pade/readme.html#special-cases)Special cases

By default, Pade should run out of the box with Openfire default settings. However, if ldap or any other custom user provider is being used, user accounts must be created manually for jvb, focus and jigasi (if needed) as the plugin cannot do this automatically.

On Windows servers, Pade may not work if Openfire is installed in the default location **"Program Files/Openfire"** **Place it in ``C:\Openfire``  because of the embedded space in the name it **WILL cause issues**. Try using a different location with no embedded spaces. Also note that Jitsi videobridge cannot use the webrtc datachanel because of a missing binary in Windows and **must** use websockets for the data channel to Jitsi Meet. Port 8180 will be used by default in Openfire. A websocket proxy has been implemented in Pade to proxy from the configured Openfire websocket TLS port (7443) to 8180. This allows JVB2 to reuse the Openfire domain certificate for TLS on port 7443.

If port 8180 is in use elsewhere then this needs to be changed. Use the Network web page to do so. If you use iptables, an external web server like nginx or haproxy to redirect standard TLS port 433 to Openfire TLS port 7443, then the public 'advertised port' for websockets (the publicly-accessible port Jitsi Meet web client will use) should be set to 443. Otherwise leave the default value as your Openfire TLS port (7443).

[![image](https://user-images.githubusercontent.com/110731/102720510-ae5d5780-42ec-11eb-9531-2e4b9a9523e8.png)](https://user-images.githubusercontent.com/110731/102720510-ae5d5780-42ec-11eb-9531-2e4b9a9523e8.png)

If you want to allow regular telephone users to join a conference from a home or office telephone, you would need to set up the SIP Gateway to a telephone provider. You would need to script an IVR (interective response) which will allow the caller to use the phone buttons/touch tones to select their destination meeting and convert that into a room name in the SIP header that Jigasi will use to route the call to the appropriate meeting room. For an example, see [https://voximplant.com/docs/tutorials/jigasi-setup](https://voximplant.com/docs/tutorials/jigasi-setup)

The alternative is much simpler if you already have FreeSWITCH with working phones and trunks setup with an external telephone line provider. You enable the Pade to connect to FreeSWITCH via ESL (external socket library) and Pade will start to monitor every meeting. When the focus user joins, it will create a FreeSWITCH audio conference and initiate a call from the audio conference to Jigasi adding a SIP header with the name of the meeting room. You can now update your FreeSWITCH dial plan with internal and external telephone numbers that can be used by your users to to join the FreeSWITCH audio conference that is bridged to the Jitsi meeting.
