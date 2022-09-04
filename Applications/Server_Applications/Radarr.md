# Radarr

- Radarr
	- https://radarr.video/
	- Manage, View, and download you movie Libary
	- on 31 Jan 2022 after an upgrade to version 4.0.4.5922 I had a corrupted database. I fixed it with the following command  './sqlite3 radarrold.db ".recover" | ./sqlite3 radarr.db' I found the file in /programdata folder and I changed the .db file name from radarr.db to radarrold.db before running the command. Then it booted just fine!