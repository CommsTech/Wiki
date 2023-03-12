# Drive Replacement

## Replacing drives with old SAS drives
Video: https://youtu.be/msl9FVGRSdg
Here is my 2 min walkthrough of a 14 hour solution to the following problem.
My Original Error: [EFAULT] Unable to determine size of 'sde'

Steps to repair:
1) identify the Hard drive 
	Storage -> pool -> select the vde
	(in my case the drive was sde)
2) Run the following command and confirm that your new drive is formatted for a sector size of 520
	sg_readcap -l /dev/sdX (Where "X" = your drive letter in my case it was sde)
	example: sg_readcap -l /dev/sde

3) AFTER confirming you have the same issue run the following command to change your sector size to 512
	sg_format -F -s 512 /dev/sg
	example: sg_format -F -s 512 /dev/sde


Resources: 
https://Wiki.commsnet.org
https://www.truenas.com/community/threads/troubleshooting-disk-format-warnings-in-bluefin.106051/


