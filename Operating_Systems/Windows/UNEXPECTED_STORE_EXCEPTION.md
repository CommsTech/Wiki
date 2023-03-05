---
title: Unexpected Store Exception
description: 
published: true
date: 2022-07-25T13:15:54.706Z
tags: 
editor: markdown
dateCreated: 2022-05-21T15:28:21.146Z
---
# Unexpected Store Exception
## What Is the “Unexpected Store Exception” Error in Windows?

The “Unexpected Store Exception” error is caused by various hardware and file integrity issues associated with the kernel memory module. The bug is also known as _UNEXPECTED_STORE___EXCEPTION an_d has a value of 0x00000154, which refers to a “stop code,” a special category of blue screen errors.

This kernel memory module manages the RAM used by some core components in Windows as well as device drivers. The Windows operating system uses it to talk to device hardware.

You can view your device kernel memory in the [Task Manager](https://www.maketecheasier.com/open-task-manager-windows/) under the Memory tab. Compared to physical RAM, kernel memory is only a few hundred MB in size. It is further classified according to paged and non-paged pools.

![Unexpected Store What Is Kernel Memory](https://www.maketecheasier.com/assets/uploads/2022/11/Unexpected-Store-What-is-Kernel-Memory.jpg)

Anyway, here’s how to start tackling the “Unexpected Store Exception” error in Windows.

## 1. Debug With WinDbg

To debug the “Unexpected Store Exception” error, we are using a Microsoft debugging program called WinDbg. While it involves a little bit of coding, it is very easy to learn.

### WinDbg Download and Installation

WinDbg, the official Windows debugger, can be [downloaded from this link](https://developer.microsoft.com/en-us/windows/downloads/sdk-archive/) as part of Windows Software Development Kit (SDK). For installation, choose the “Install SDK” file based on your operating system and latest version number. Windows 11 users have it relatively easy. If you’re a Windows 10 user, it’s better to update your system first, then install the latest SDK.

![Unexpected Store Windows Sdk Downloads Install Sdk](https://www.maketecheasier.com/assets/uploads/2022/05/Unexpected-Store-Windows-SDK-Downloads-Install-SDK.jpg)

1.  After downloading the executable file, install it on your system by specifying a location or choose the automatic install path. Follow the on-screen instructions.

![Unexpected Store Install Winsdk In Windows11](https://www.maketecheasier.com/assets/uploads/2022/11/Unexpected-Store-Install-WinSDK-in-Windows11.jpg)

2.  This step is very important. For debugging your Windows software, you don’t need all the features of the Windows SDK. You only need to check “Debugging Tools for Windows” and deselect everything else.

![Unexpected Store Winsdk Components To Install](https://www.maketecheasier.com/assets/uploads/2022/11/Unexpected-Store-WinSDK-Components-to-Install.jpg)

3.  Wait a few minutes for the installation to complete, as Windows SDK acquires debug tools for Windows.

![Unexpected Store Installing Windbg](https://www.maketecheasier.com/assets/uploads/2022/11/Unexpected-Store-Installing-WinDbg.jpg)

4.  You will see a final welcome message once Windows SDK is installed on your device.

![Unexpected Store Installation Successful Winsdk](https://www.maketecheasier.com/assets/uploads/2022/11/Unexpected-Store-Installation-Successful-WinSDK.jpg)

5.  Locate the installation directories depending on whether you have an x64 [“C:Program Files (x86)Windows Kits10Debuggersx64”] or x86 [“C:Program Files (x86)Windows Kits10Debuggersx86”] system. You can check your CPU configuration in “About your PC” from the Windows search menu.

![Unexpected Store Windbk Locate Installer X64](https://www.maketecheasier.com/assets/uploads/2022/11/Unexpected-Store-WinDbK-Locate-Installer-x64.jpg)

6.  Go inside the installation directory and locate the WinDbg executable file as shown below for the x64 folder.

![Unexpected Store Debuggers Locate Windbg File](https://www.maketecheasier.com/assets/uploads/2022/11/Unexpected-Store-Debuggers-Locate-Windbg-File.jpg)

7.  Double-click to launch the WinDbg executable. From the “File -> Open Executable” menu, you can view any executable file on your PC.

![Unexpected Store Windbg Open Executable](https://www.maketecheasier.com/assets/uploads/2022/11/Unexpected-Store-WinDbg-Open-Executable.jpg)

8.  Locate the “Notepad.exe” file on your system; it is usually based in the System32 folder. Open this executable using WinDbg.

![Unexpected Store Windbg Open Executable Notepad](https://www.maketecheasier.com/assets/uploads/2022/11/Unexpected-Store-WinDbg-Open-Executable-Notepad.jpg)

9.  To test this debugger, enter `.sympath srv*` in the WinDbg command line at the bottom.

![Unexpected Store Windbg Command Line Using Sympath Srv Command](https://www.maketecheasier.com/assets/uploads/2022/11/Unexpected-Store-WinDbg-Command-Line-Using-Sympath-Srv-Command.jpg)

10.  The output will be shown in the [Notepad window](https://www.maketecheasier.com/add-open-with-notepad-to-context-menu-windows/). Below is a symbol search path that tells WinDbg where to look for symbol (PDB) files.

Symbol search path is: srv*
Expanded Symbol search path is: cache*;SRV

![Unexpected Store Windbg Command Line Output Sympath Srv Command](https://www.maketecheasier.com/assets/uploads/2022/11/Unexpected-Store-WinDbg-Command-Line-Output-Sympath-Srv-Command.jpg)

**Tip**: learn how to [add “Open with Notepad” to the context menu](https://www.maketecheasier.com/add-open-with-notepad-to-context-menu-windows/) in Windows.

### Option 1: Create a User Mode Dump File

After downloading and installing WinDbg, use “Crash mode” analysis to solve the Exception error. This involves causing a fake crash and noticing the Dump file as shown below.

1.  Open Settings on your PC.
2.  On the right side, scroll down and select “About.”

![Unexpected Store Error Message About](https://www.maketecheasier.com/assets/uploads/2022/05/Unexpected-Store-Error-Message-About.jpg)

3.  Select “Advanced System Settings.”

![Unexpected Store Error Message Advanced System Settings](https://www.maketecheasier.com/assets/uploads/2022/05/Unexpected-Store-Error-Message-Advanced-System-Settings.jpg)

4.  This will open a “System Properties” window.
5.  Select the Advanced tab at the top and press “Settings” under “Startup and Recovery.”

![Unexpected Store System Properties Settings](https://www.maketecheasier.com/assets/uploads/2022/11/Unexpected-Store-System-Properties-Settings.jpg)

6.  A new “Startup and Recovery” window will open.
7.  Take note of the location of the “Dump file.” It can be easily overwritten without any issues.

![Unexpected Store Dump File Startup Recovery](https://www.maketecheasier.com/assets/uploads/2022/05/Unexpected-Store-Dump-File-Startup-Recovery.jpg)

8.  Go back to the WinDbg command line and enter the following to create a user mode dump file.

.dump [options] FileName
.dump /?

You can also operate it as a kernel mode dump, but the procedure is different and unnecessary.

9.  Instead of “?,” you’ll have to enter `mf` or `ma`, which refer to a variety of dump files. Also, replace “FileName” with the Dump file path used earlier.
10.  Your exception analysis has begun. If there are any exception errors on your system, they will be highlighted here. The problematic files will be mentioned in the log, and you can take corrective action from there.

![Unexpected Store Create Dump File User Kernel Mode](https://www.maketecheasier.com/assets/uploads/2022/05/Unexpected-Store-Create-Dump-File-User-Kernel-Mode.jpg)

11.  Apart from WinDbg, another method to create a fake crash is [“NotMyFault” by SysInternals](https://docs.microsoft.com/en-us/sysinternals/downloads/notmyfault). Download the zipped file and extract it.
12.  Click on the NotMyFault application to launch a new window to emulate a crash. Close all of the other applications so that corrupted memory is not written to the disk after Restart.

![Unexpected Store Not My Fault](https://www.maketecheasier.com/assets/uploads/2022/05/Unexpected-Store-Not-my-Fault.jpg)

### Option 2: Using the !Analyze Extension Command

To find the root cause of a “Store Exception” error, you can use another WinDbg command: [“!Analyze” extension](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/-analyze).

1.  Go back to the WinDbg command line and copy-paste the following:

!analyze [-v] [-f | -hang] [-D BucketID]
!analyze -c [-load KnownIssuesFile | -unload | -help ]

![Unexpected Store Analyze Command User Mode Entered](https://www.maketecheasier.com/assets/uploads/2022/05/Unexpected-Store-Analyze-Command-User-Mode-Entered.jpg)

2.  The exception analysis will start. If there are any problem files on your PC, they will be recorded in the log.
3.  Remove those files or uninstall any programs associated with the “Store Exception” error. If there aren’t any issues, you won’t see any results.

![Unexpected Store Analyze Exception Analysis Results Unknown Option](https://www.maketecheasier.com/assets/uploads/2022/05/Unexpected-Store-Analyze-Exception-Analysis-Results-Unknown-Option.jpg)

4.  The “Bucket ID” refers to the exact event ID connected to the “Store Exception” error.
5.  As soon as you notice a blue screen on your laptop display, you can see the “Bucket ID” highlighted. If you miss recording it, you can find the same “Bucket ID” from “Event Viewer” in Windows.

![Unexpected Store Event Viewer Event Bucket Id](https://www.maketecheasier.com/assets/uploads/2022/11/Unexpected-Store-Event-Viewer-Event-Bucket-ID.jpg)

While WinDbg is the best method to debug the “Unexpected Store Exception” problem, if you don’t want to learn the programming, Microsoft recommends a few other options.

## 2. Perform System Restore Based on Update History

If any blue screen errors are caused by some recent events, you can perform a simple System Restore to take your Windows device to an earlier configuration.

1.  From the Windows search menu, open “View your Update history.”

![Unexpected Store View Update History](https://www.maketecheasier.com/assets/uploads/2022/11/Unexpected-Store-View-Update-History.jpg)

2.  Look for a recent event from a few days before you noticed the “Store Exception” error. Its date will be used to send the system to an earlier point of time.

![Unexpected Store Update History Recent Event](https://www.maketecheasier.com/assets/uploads/2022/11/Unexpected-Store-Update-History-Recent-Event.jpg)

3.  Open “Create a restore point” from a Start menu search. Go to its “System Protection” tab and click “System Restore.”

![Unexpected Store Create A Restore Point](https://www.maketecheasier.com/assets/uploads/2022/11/Unexpected-Store-Create-a-Restore-Point.jpg)

4.  Check the “Show more restore points” option at the bottom and select the event date you want to return to.

![Unexpected Store System Restore Go Back Old Event](https://www.maketecheasier.com/assets/uploads/2022/11/Unexpected-Store-System-Restore-Go-Back-Old-Event.jpg)

5.  Confirm your restore point, and the device will restart to return to the older configuration.

![Unexpected Store Confirm Restore Point](https://www.maketecheasier.com/assets/uploads/2022/11/Unexpected-Store-Confirm-Restore-Point.jpg)

## 3. Use Windows Memory Diagnostic

You can use the Windows Memory Diagnostic app to take care of bad memory sectors behind the “Unexpected Store Exception” crash.

1.  Launch Windows Memory Diagnostic from the Windows search menu.

![Unexpected Store Memory Diagnostic Start](https://www.maketecheasier.com/assets/uploads/2022/11/Unexpected-Store-Memory-Diagnostic-Start.jpg)

2.  As soon the app is launched, you will be asked to restart the device and check for memory problems.
3.  You can either restart your PC straightaway or schedule the diagnostics for any other time.

![Unexpected Store Memory Diagnostic Restart Check](https://www.maketecheasier.com/assets/uploads/2022/11/Unexpected-Store-Memory-Diagnostic-Restart-Check.jpg)

## 4. Look for Exclamation Point in Device Manager Hardware

If hardware issues are causing the blue screen problem, you may need to replace the older or outdated hardware. Trace glitchy hardware using the steps below.

1.  [Open Device Manager](https://www.maketecheasier.com/ways-open-device-manager-windows/) from Start menu search or `devmgmt.msc` from the “Run” command.

![Unexpected Store Device Manager](https://www.maketecheasier.com/assets/uploads/2022/11/Unexpected-Store-Device-Manager.jpg)

2.  Under each hardware device, look for exclamation points (!). If you have any faulty external webcams, SD cards, pen drives, or external drives, remove them first. A worn-out battery should be replaced quickly, as it can lead to major issues.

![Unexpected Store Device Manager Exclamation Points](https://www.maketecheasier.com/assets/uploads/2022/11/Unexpected-Store-Device-Manager-Exclamation-Points.jpg)

## 5. Perform System File Checker Scan and ChkDsk

The best way to restore the integrity of your Windows system is to use the System File Checker (SFC) tool. It helps repair missing or corrupted system files.

1.  Open Command Prompt from the Windows search menu and run it as administrator.
2.  Run a basic SFC scan using `sfc /scannow`.
3.  If any system repair restart is pending, go ahead with it and run SFC again.

![Unexpected Store Sfc Scannow](https://www.maketecheasier.com/assets/uploads/2022/05/Unexpected-Store-SFC-Scannow.jpg)

4.  You can also use the ChkDsk utility to repair any problems in the hard disk.

chkdsk /f /c:

![Unexpected Store Chkdsk Results](https://www.maketecheasier.com/assets/uploads/2022/05/Unexpected-Store-Chkdsk-Results.jpg)

## 6. Remove Temporary Files

Sometimes the cure for a store exception error is to delete the temporary files that may be clogging up your computer.

1.  Type Disk Cleanup in the search bar.
2.  Open Disk Cleanup.

![Unexpected Store Disk Cleanup Search Menu 1](https://www.maketecheasier.com/assets/uploads/2022/11/Unexpected-Store-Disk-Cleanup-Search-Menu-1.jpg)

3.  Use the drop-down arrow to select the drive you want to clean. This will usually be C:.

![Unexpected Store Disk Cleanup Temporary Files](https://www.maketecheasier.com/assets/uploads/2022/11/Unexpected-Store-Disk-Cleanup-Temporary-Files.jpg)

4.  Click OK.
5.  Wait for the list of files to appear.
6.  Scroll down to find “Temporary files” and check the box next to it. You may also want to check “Temporary Internet files.”
7.  Click “OK” and confirm you want to delete the files.

## 7. Disable Fast Startup

Fast startup is enabled by default on your device to start your device more quickly by using a particular hibernation option. Using fast startup can cause the store exception error, so try disabling it to see if that solves the problem.

1.  Type “Control Panel” in the Search box.
2.  Open Control Panel.

![Unexpected Store Control Panel](https://www.maketecheasier.com/assets/uploads/2022/11/Unexpected-Store-Control-Panel.jpg)

3.  Click “Change what the power buttons do” under “System and Security -> Power Options.”

![Unexpected Store Control Panel System And Security Power Options](https://www.maketecheasier.com/assets/uploads/2022/11/Unexpected-Store-Control-Panel-System-and-Security-Power-Options.jpg)

4.  Click on “Change settings that are currently unavailable.”
5.  Untick the box next to “Turn on fast startup.”
6.  Click “Save changes.”

![Unexpected Store Control Panel Turn Off Fast Startup](https://www.maketecheasier.com/assets/uploads/2022/11/Unexpected-Store-Control-Panel-Turn-Off-Fast-Startup.jpg)

**Tip**: learn how Fast Startup [differs from other Windows power settings](https://www.maketecheasier.com/shut-down-vs-sleep-vs-hibernate/).

## 8. Update Your Display Drivers

Another option that may solve the store exception error is updating your display drivers. First enter Safe mode so that your current display doesn’t become blank during the update process.

1.  Press Win + I to open Settings.
2.  Go to the “Recovery” menu under “System.”
3.  Click “Restart Now” next to “Advanced Startup” to restart your Windows 10 or 11 PC in Safe mode.
4.  After your PC restarts and displays the “Choose an option” screen, select “Troubleshoot -> Advanced options -> Startup Settings -> Restart.” It may ask you for your BitLocker recovery key, which is found in your Microsoft account online.

![Unexpected Store Advanced Startup Restart Now](https://www.maketecheasier.com/assets/uploads/2022/11/Unexpected-Store-Advanced-Startup-Restart-Now.jpg)

5.  After your PC restarts, you’ll see a list of options. You’ll most likely need to use the Internet, so select 5 or press F5 for Safe Mode with Networking.

After you have entered Safe mode:

1.  Open Device manager by typing `devmgr` in the search box.
2.  In the list of devices that appear, locate “Display adapters” and double-click it.
3.  Right-click the available display adapter and click “Update driver.” Restart your computer for the new driver effects to be updated.

![Unexpected Store Display Manager Update](https://www.maketecheasier.com/assets/uploads/2022/11/Unexpected-Store-Display-Manager-Update.jpg)

## Frequently Asked Questions

### How can I fix unexpected store exception errors during gaming?

If you encounter an unexpected store exception and other bluescreen errors in the middle of a game, you will need to use the Windows Debugger software (WinDbg) that analyzes the mini dumps created by the errors.

### How is the the unexpected store exception "boot device not found" fixed?

If you noticed an unexpected store exception error along with a “boot device not found” message, it means your computer may have BIOS issues that are preventing the Windows operating system from being used as the boot device.

To solve the problem, open your computer’s BIOS page. The shortcut to access it varies from manufacturer to manufacturer. (For example, on Dell laptops, you have to hit the F2 key repeatedly to get access to the BIOS screen.)

On the BIOS page, ensure that the SATA configuration is set to AHCI. Also, ensure that Windows has been set as the boot device. Once you restart your device, the error should take care of itself.

### Can blue screens happen for no reason?

The main causes of blue screen errors are hardware issues as well as corrupt drivers. They do not happen unexpectedly, and you can always pinpoint any blue screen error to one of the recent event IDs in Event Viewer.

We’ve outlined some of the most effective methods to troubleshoot the _UNEXPECTED_STORE_EXCEPTION_ error in Windows, which is just one example of a blue screen error. We can also help you with other kinds of blue screen issues, such as the [Kernel Data InPage error](https://www.maketecheasier.com/fix-windows-kernel-data-inpage-error/) and the [Critical Process Died error](https://www.maketecheasier.com/fix-critical-process-died-windows/).