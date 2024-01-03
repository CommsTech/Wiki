---
title: Plexinator
description: Convert your media to play nicely with plex
dateCreated: 2022-08-30T04:27:17.152Z
published: true
editor: markdown
tags:
  - Application
  - Media
  - Management
dateModified: 
---
# Plexinator

- Plexinator
	- This is a little project i started awhile back that will go through your media collection and make it compatible H264 and Optimized for streaming.

Prereqs  

1. Handbreakcli.exe https://handbrake.fr/downloads2.php  
2. ffmpeg.exe https://ffmpeg.org/download.html 
3. ffprobe.exe https://ffmpeg.org/download.html  
4. filebot.exe https://www.filebot.net/#download  
5. VideoDuplicatFinder https://github.com/0x90d/videoduplicatefinder  
6. Video files to optimize  

Plexinator_Setup_Wizard.bat (Used to set variables and download prereqs)  
Must run Plexinator_Setup_Wizard.bat prior to using Plexinator.bat  

Plexinator.bat (The script that does the stuff)  
Step 1 : Set Working Directory  
Step 1.1: (Optional) Remove Duplicates  
Step 2 : HandBreakcli Conversion from ts,m4v,mov,avi,flv,Mpeg to MP4 Web optimised  
        * Note All Handbreak Converted Videos will replace the originals in the originals folder. (this will delete the original)  
Step 2.1 : MKV to MP4 Conversion  
Step 3 : FFMPEG to Optimize existing .MP4 files (current issue is all .mp4's get optimised even if they dont need it | 14 Feb 2020)  
        * Note All FFMPEG REMUXED Videos will replace the originals in the originals folder. (this will delete the original)    

Step 4 : Filebot to rename converted files  
      * Note Filebot gets the name wrong often so i find myself correcting this issue so currently Filebot spits out the renamed  
           Files into the Output directory  
Step 5 : PNRxA Script utilizing handbreak to test media library  

  

Library Validator script pulled from: https://github.com/PNRxA/corrupted-media-scanner  

  
  

Feel free to submit issues, commit changes and/or fork and make this better!  

  

  (Update 30 Jan 2020 - Just found an amazing de duplicator and working to integrate it into another option. Will soon work on migrating this from a cli automator to a GUI for ease of access)

  

[Plexinator Source Github Repo](https://github.com/CommsTech/Plexinator)
