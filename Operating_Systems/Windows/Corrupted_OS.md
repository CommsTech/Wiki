Using SFC and DISM is often done in haste and incorrectly. If you need to fix a corrupted install, then a special DISM command MUST BE issued.

## The Commands

### Basic Online Command

![](https://d33wubrfki0l68.cloudfront.net/861a44aa4ed43e664d6ea626eeefda449bd2d2ac/73bc0/images/2022/fix-corrupt-windows-install/dism-normal.png)

```fallback
DISM /Online /Cleanup-Image /CheckHealthDISM /Online /Cleanup-Image /CheckHealth
```

Copy

_Note: This will check it’s health and cleanup basic corruption errors._

### Command to Fix From Windows ISO

Use this command when the basic one fails. Follow these steps:

1.  Download the Windows ISO from [https://www.microsoft.com/en-us/software-download/windows10ISO](https://www.microsoft.com/en-us/software-download/windows10ISO)
2.  Mount the ISO and note the drive letter (ex. E:)
3.  Run DISM with sources flag

```fallback
DISM /Online /Cleanup-Image /RestoreHealth /Source:E:\Sources\install.wim
```

Copy

_Note: `install.wim` is known as ESD in some downloads `install.esd`_

## Verify History and Logs

Did it run correctly? Was the corruption repaired?

Check the log file at `%windir%\Logs\DISM\dism.log`

## SFC - The Worthless Tool

System file checker is WORTHLESS! In the best scenario it might tell you about some corruption, but I have never seen it actually repair anything.

Yet every damn guide on the internet recommends you run it. Save your time, and use DISM instead.

## Walkthrough Video

[#DISM](https://christitus.com/tags/dism)