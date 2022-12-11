---
title: Dell Venue 11 Pro
description: All the Dell_Venue_11_Pro stuff in one place
published: true
date: 2022-11-30T12:56:37.752Z
tags: Dell, Tablet
editor: markdown
dateCreated: 2022-05-16T14:19:22.595Z
---

# Dell Venue 11 Pro


Original Article located at https://www.dell.com/community/Tablets-Mobile-Devices/Venue-11-pro-i5-4300y-Throttling/m-p/4404025#M24903

Download the custom boot file bootx64.efi and then copy it to X:\\efi\\boot\\ then place in a USB thumb drive.

Secondly disable secure boot in bios

Then boot with the USB thumb drive, you should be in a mini console. Then at the prompt enter the variables you want to edit. eg.

grub> setup\_var {address} {new value}

The mods.txt will show you the addresses and values to change, ie the address, the default value and then the desired value.

eg

grub> setup\_var 0x39  0x00     

the first is the address, second its default value and last is the value you need to change it to.

\* 0xD0E        0x01        0x00 ; Package power limit lock  
\* 0x54        0x01        0x00 ; Platform power limit lock  
\* 0xD04        0x01        0x00 ; CFG lock  
\* 0xD0D        0x01        0x00 ; VR Current value lock  
\* 0x39        0x00        0x00 ; Configurable TDP lock  
\* 0x23        0x20        0x10 ; TCC activation offset

\* 0x3A        0x00        0x10 ; Config TDP lock

These modifications are reversible.

Also you can edit the power setting for increasing the Turbo boost max using a program called bar edit. Remember to install the tvicport first.

When in bar edit locate the address FED159A0 and change all the values to 0000000

There it is done no more throttling.

ps The tablet has to be fully charged or on battery for effect to take place.

links below

[_www.mediafire.com/.../TVicPortInstall41.exe_](http://www.mediafire.com/download/flgx1f10apnwbtq/TVicPortInstall41.exe)

[_www.mediafire.com/.../bootx64.efi_](http://www.mediafire.com/download/esxhvfshpxdy870/bootx64.efi)

[_www.mediafire.com/.../BAR-Edit.exe_](http://www.mediafire.com/download/b6w63lvf44dvp07/BAR-Edit.exe)

[_www.mediafire.com/.../mods.txt_](http://www.mediafire.com/view/dkmahso8xbze4ax/mods.txt)

https://fohdeesha.com/docs/H710P-B0.html