
I am still using the original version and I a hoping all of this will translate right over to the the new version which I have just started testing.

The goal is to have pfsense still running IDS where it can actively block threats but still export data over to Security Onion. I will cover the port mirror to the SO sensor as part of the tutorial as well but here is what I have so far for exporting data:

**in pfsense**

- In pfSense navigate to Status->System Logs, then click on Settings.
- At the bottom check "Enable Remote Logging"
- Enter the Security Onion local IP into the field "Remote log servers" with port 514 (eg 192.168.2.8:514)
- Under "Remote Syslog Contents" check "Everything"

*The root cause of most issues was using the Syslog format PFsense logs instead of the default BSD format. *

**Suritcata-in pfsense settings see** [https://i.imgur.com/oRWxJOh.png](https://i.imgur.com/oRWxJOh.png)

- Interfaces: For each interface you have configured, edit and repeat steps for each interface
- In each "Interface" Settings -> under Alert Settings check Send Alerts to System Log
- "Log Facility" should be "LOCAL1" & "Log Priority" should be "NOTICE"
- Further down under "EVE Output Settings", check "EVE JSON Log"
- "EVE Output Type" set to "SYSLOG" "EVE Syslog Output Facility" set to "AUTH" and "EVE Syslog Output Priority" set to "notice"

**For Security Onion**

- "sudo so-allow" choose [l] "Syslog Device - port 514" and allow the pfsesne IP address

While these settings are working and SO is ingesting logs from pfsense, I am wondering is what other settings should I change that would be more optimal or if I have overlooked anything. Also, pfsense offers the Telegraf package which can export directly to the Elastic Search port 9200 but I am not as clear on what data would be exported and if it would be any more useful that sending over the syslogs.

Here are some screenshots for reference [https://imgur.com/a/2RsVyxU](https://imgur.com/a/2RsVyxU)

Upvote18Downvote5commentsShare