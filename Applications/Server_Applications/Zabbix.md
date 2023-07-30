---
title: Zabbix
description: Zabbix Network Monitoring Service
published: true
date: 2022-07-25T13:10:25.579Z
tags: ubuntu, networking, routing, snmp
editor: markdown
dateCreated: 2022-05-16T14:46:52.356Z
---

## Install Zabbix Server on Ubuntu 20.04|18.04


- MySQL/ MariaDB database server

MySQL or MariaDB can be a remote server, but php and httpd need to be installed on the Zabbix server. Follow steps below to have Zabbix server installed and working on your Ubuntu system.

# Install and configure Zabbix on Ubuntu 22.04

### Step 1: Install Zabbix repository

[Documentation](https://www.zabbix.com/documentation/6.4/manual/installation/install_from_packages)

`# wget https://repo.zabbix.com/zabbix/6.4/ubuntu/pool/main/z/zabbix-release/zabbix-release_6.4-1+ubuntu22.04_all.deb   # dpkg -i zabbix-release_6.4-1+ubuntu22.04_all.deb   # apt update`

### Step 2: Install Zabbix server, frontend, agent

`# apt install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-sql-scripts zabbix-agent   `


### Step 3: Install MariaDB Database Server

Run the commands below to install MariaDB database server:

```
sudo apt update
sudo apt install mariadb-server
```

Once Database server installation is done open MySQL terminal

```
$ sudo mysql -u root
Welcome to the MariaDB monitor. Commands end with ; or \g.
Your MariaDB connection id is 56
Server version: 10.3.7-MariaDB-1:10.3.7+maria~bionic-log mariadb.org binary distribution
Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.
Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
```

Create a database and user for Zabbix:

```
CREATE DATABASE zabbix character set utf8 collate utf8_bin;;
GRANT ALL PRIVILEGES ON zabbix.* TO zabbix@'localhost' IDENTIFIED BY 'StronDBPassw0rd';
FLUSH PRIVILEGES; 
QUIT 
```

### Step 4: Configure and Start Zabbix Server

Zabbix configuration file is located under  _**/etc/zabbix/zabbix_server.conf**_. Ensure the following lines are configured correctly.

```
DBName=zabbix
DBUser=zabbix
DBPassword=StronDBPassw0rd
```

Restart Zabbix server after modifying this file:

```
sudo systemctl restart zabbix-server
```

Configure Zabbix agent to monitor Zabbix server itself.

```
$ sudo vim /etc/zabbix/zabbix_agentd.conf
Hostname=zabbix-server.commstech.org
```

#### Step 4: Optional Configure Firewall

If you have ufw firewall installed and running on your system, ensure you allow port 5000 and port 5001

```
sudo ufw allow proto tcp from any to any port 10050,10051
```

### Step 5: Perform Zabbix initial setup

Access “**http://(Zabbix server’s hostname or IP address)/zabbix/**”  to begin Zabbix initial setup.

Step 1 is a welcome page, click “**Next step**” to proceed.

[![Install and Configure Zabbix 6.0 LTS on RHEL 8 CentOS Stream 80A](https://computingforgeeks.com/wp-content/uploads/2022/01/Install-and-Configure-Zabbix-6.0-LTS-on-RHEL-8-CentOS-Stream-80A.png?ezimgfmt=rs:696x449/rscb23/ng:webp/ngcb23 "How To Install Zabbix Server on Ubuntu 20.04|18.04 1")](https://computingforgeeks.com/wp-content/uploads/2022/01/Install-and-Configure-Zabbix-6.0-LTS-on-RHEL-8-CentOS-Stream-80A.png?ezimgfmt=rs:696x449/rscb23/ng:webp/ngcb23)

Confirm that all pre-requisites are satisfied.

[![zabbix setup 02](https://computingforgeeks.com/wp-content/uploads/2018/06/zabbix-setup-02.png?ezimgfmt=rs:696x412/rscb23/ng:webp/ngcb23 "Install Zabbix Server on Ubuntu 18.04 2")](https://computingforgeeks.com/wp-content/uploads/2018/06/zabbix-setup-02.png)

Configure DB settings as added before:

[![zabbix setup 03](https://computingforgeeks.com/wp-content/uploads/2018/06/zabbix-setup-03.png?ezimgfmt=rs:696x422/rscb23/ng:webp/ngcb23 "Install Zabbix Server on Ubuntu 18.04 3")](https://computingforgeeks.com/wp-content/uploads/2018/06/zabbix-setup-03.png)

Confirm Hostname and Port number for Zabbix server. It is okay to use localhost in place of name.

[![zabbix setup 04](https://computingforgeeks.com/wp-content/uploads/2018/06/zabbix-setup-04.png?ezimgfmt=rs:696x423/rscb23/ng:webp/ngcb23 "Install Zabbix Server on Ubuntu 18.04 4")](https://computingforgeeks.com/wp-content/uploads/2018/06/zabbix-setup-04.png)

Verify all settings and click Next step to finish the initial setup.

[![zabbix setup 05](https://computingforgeeks.com/wp-content/uploads/2018/06/zabbix-setup-05.png?ezimgfmt=rs:696x433/rscb23/ng:webp/ngcb23 "Install Zabbix Server on Ubuntu 18.04 5")](https://computingforgeeks.com/wp-content/uploads/2018/06/zabbix-setup-05.png)

If all goes well, you should get congratulations page. Click the **Finish** button to end installation.

[![zabbix setup 06](https://computingforgeeks.com/wp-content/uploads/2018/06/zabbix-setup-06.png?ezimgfmt=rs:696x417/rscb23/ng:webp/ngcb23 "Install Zabbix Server on Ubuntu 18.04 6")](https://computingforgeeks.com/wp-content/uploads/2018/06/zabbix-setup-06.png)

You’ll then get the login page. Default logins are:

```
Username: "Admin"
Password: "zabbix"
```

[![zabbix setup 07](https://computingforgeeks.com/wp-content/uploads/2018/06/zabbix-setup-07.png?ezimgfmt=rs:696x426/rscb23/ng:webp/ngcb23 "Install Zabbix Server on Ubuntu 18.04 7")](https://computingforgeeks.com/wp-content/uploads/2018/06/zabbix-setup-07.png)

Default dashboard page is as below:

[![zabbix setup 08](https://computingforgeeks.com/wp-content/uploads/2018/06/zabbix-setup-08.png?ezimgfmt=rs:696x328/rscb23/ng:webp/ngcb23 "Install Zabbix Server on Ubuntu 18.04 8")](https://computingforgeeks.com/wp-content/uploads/2018/06/zabbix-setup-08.png)

### Step 6: Change Admin Password

Login to Zabbix admin dashboard with **admin** user and password **zabbix.** You need to change the password for admin user after the first login for security reasons.

Navigate to **Administration > Users > Admin > Password > Change Password**

[![zabbix setup chnage password](https://computingforgeeks.com/wp-content/uploads/2018/06/zabbix-setup-chnage-password.png?ezimgfmt=rs:696x312/rscb23/ng:webp/ngcb23 "Install Zabbix Server on Ubuntu 18.04 9")](https://computingforgeeks.com/wp-content/uploads/2018/06/zabbix-setup-chnage-password.png)

Enter the new password twice then click on the Update button to change.

### Step 7: Configure Monitoring Target host

Now that we have our Zabbix server ready for monitoring, let’s configure first monitoring target host – This is Zabbix server monitoring itself.

Login to Zabbix admin dashboard with the username **admin** and click on **Configuration > Hosts.** You should have seen that the host localhost status is set to “**Disabled”.**

[![zabbix setup 09](https://computingforgeeks.com/wp-content/uploads/2018/06/zabbix-setup-09.png?ezimgfmt=rs:696x225/rscb23/ng:webp/ngcb23 "Install Zabbix Server on Ubuntu 18.04 10")](https://computingforgeeks.com/wp-content/uploads/2018/06/zabbix-setup-09.png)

Click on the disabled button to enabled Zabbix agent on this server to monitor the host.  The “Status” is turned to “**enabled**” and the server is now being monitored.

[![zabbix setup 10](https://computingforgeeks.com/wp-content/uploads/2018/06/zabbix-setup-10.png?ezimgfmt=rs:696x260/rscb23/ng:webp/ngcb23 "Install Zabbix Server on Ubuntu 18.04 11")](https://computingforgeeks.com/wp-content/uploads/2018/06/zabbix-setup-10.png)

After few minutes, monitoring data will start flowing in, to check host graphs go to **Monitoring > Screens > Server name**

[![zabbix setup 11](https://computingforgeeks.com/wp-content/uploads/2018/06/zabbix-setup-11.png?ezimgfmt=rs:696x344/rscb23/ng:webp/ngcb23 "Install Zabbix Server on Ubuntu 18.04 12")](https://computingforgeeks.com/wp-content/uploads/2018/06/zabbix-setup-11.png)


##### Some of my screenshots

![[Pasted image 20230729162412.png]]


mysql -uroot -p
asdasHJGGJ687i_hkjHG^
create database zabbix character set utf8mb4 collate utf8mb4_bin;
create user zabbix@commsnet.org identified by 'asdasHJGGJ687i_hkjHG^';
grant all privileges on zabbix.* to zabbix@commsnet.org;
set global log_bin_trust_function_creators = 1;
quit;


create database zabbix character set utf8mb4 collate utf8mb4_bin;
create user zabbix@localhost identified by 'asdasHJGGJ687i_hkjHG^';
grant all privileges on zabbix.* to zabbix@localhost;
set global log_bin_trust_function_creators = 1;
quit;


mysqldump -h localhost -u'root' -p'asdasHJGGJ687i_hkjHG^' --single-transaction 'zabbix' | gzip > /opt/zabbix_backup/db_files/zabbix_backup.sql.gz