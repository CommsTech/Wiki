---
title: Ansible GUI - Semaphore
description: a guide to using the Semaphore GUI (Graphical User Interface) with Ansible, an open-source configuration management and automation tool. It may include (If i got around to it) instructions on how to set up and use the Semaphore GUI for managing and visualizing Ansible jobs, builds, and deployments.
dateCreated: 
published: 
editor: markdown
tags:
  - Ansible
  - Semaphore
  - CI/CD
  - Continuous_Integration
  - Continuous_Delivery
  - DevOps
dateModified: 
---
# Ansible_Gui_Semaphore
[Red Hat](https://www.openshift.com/try?utm_content=inline-mention)‘s open source [Ansible](https://www.ansible.com/) is an open source IT automation platform, [written in Python](https://thenewstack.io/an-introduction-to-python-a-language-for-the-ages/), that can configure systems, deploy software, and orchestrate advanced workflows. By default, Ansible is a command-line tool but isn’t terribly complicated to work with.

However, there are some who’d much prefer having a graphical user interface (GUI) to make the platform more efficient to use. Thankfully, there’s one particular GUI, called Semaphore, that can help make using Ansible easier for larger environments and organizations.

I want to walk you through the process of installing [Semaphore](https://www.ansible-semaphore.com/). I’m going to demonstrate on Ubuntu Linux (version 22.04), so you’ll want to make sure to have Ansible installed and working. To do that, make sure to first follow [this tutorial](https://thenewstack.io/install-ansible-on-ubuntu-server-to-automate-linux-server-deployments/). Once you’ve taken care of that, you are ready to install Semaphore.

## What You’ll Need

Obviously, you’ll need Ansible up and running on Ubuntu. You’ll also need a user with `sudo` privileges. That’s it. Let’s get to the installation.

## Installing Semaphore

Although you can easily install Semaphore with Snap, we’re going to go a different route, so we can ensure the platform is available from anywhere on your LAN.

The first thing to do is install a database server.  We’re going to go with MariaDB. To install MariaDB on Ubuntu, you must add the repository with the command:  

```
curl -LsS https://downloads.mariadb.com/MariaDB/mariadb_repo_setup | sudo bash -s —
```
  
After that command finishes, install both the server and client with:  

```
sudo apt install mariadb-server mariadb-client -y
```
  
With MariaDB installed, secure it with the command:  

```
sudo mariadb-secure-installation
```
  
**Answer n to the first question**
Switch to unix_socket authentication [Y/n] n
**and y to the remaining**
Change the root password? [Y/n] y ( You’ll also be prompted to create and verify a root user password.)
Remove anonymous users? [Y/n] y
Disallow root login remotely? [Y/n] y
Remove test database and access to it? [Y/n] y
Reload privilege tables now? [Y/n] y


With the database installed, it’s time to add Semaphore. We’ll first set a variable for the version with the command:  

```
VER=$(curl -s https://api.github.com/repos/ansible-semaphore/semaphore/releases/latest|grep tag_name | cut -d '"' -f 4|sed 's/v//g')
```
  
We can now use that variable to download the correct version with the command:  

```
wget https://github.com/ansible-semaphore/semaphore/releases/download/v${VER}/semaphore_${VER}_linux_amd64.deb
```
  
Install Semaphore with:  

```
sudo apt install ./semaphore_${VER}_linux_amd64.deb
```
  
Boom! Semaphore is installed and ready to be configured.

## Configure Semaphore

You don’t just edit a configuration file because none exists yet. To generate the configure file, run semaphore such that it will prompt you to configure everything. The command for this is:  

```
sudo semaphore setup
```
  
The first section of the configuration looks like this:  (Select 1 (MySQL))

```
|Hello! You will now be guided through a setup to:
1. Set up configuration for a MySQL/MariaDB database
2. Set up a path for your playbooks (auto-created)
3. Run database Migrations
4. Set up initial semaphore user &amp; password

What database to use:<

1 - MySQL
2 - BoltDB
3 - PostgreSQL
(default 1):
```


  
Make sure to select MySQL for your database and then configure it accordingly. You can accept the default for everything, but you will have to type the MariaDB root user password you created earlier.

When you get to the Hostname section (which looks like db Hostname (default 127.0.0.1:3306):), make sure to type it in the form:  

```
http://ANSIBLE:3000
```

  
Where ANSIBLE is the IP address of your hosting server.

Example : db Hostname (default 127.0.0.1:3306): http://192.168.255.10:3000

db User (default root): root

db Password: SUPERSECRETPASSWORD

db Name (default semaphore): 

db Name (default semaphore): 

Playbook path (default /tmp/semaphore): 

Web root URL (optional, see https://github.com/ansible-semaphore/semaphore/wiki/Web-root-URL): 

Enable email alerts? (yes/no) (default no): no

Enable telegram alerts? (yes/no) (default no): no 

Enable slack alerts? (yes/no) (default no): no

Enable LDAP authentication? (yes/no) (default no): no

Config output directory (default /home/commstech):

Near the end of the prompt, you’ll also be asked to create a new admin user for the web UI.

## Create a Systemd File

Next, we need to create a systemd file so the Semaphore service can be controlled. Create the file with the command:  

```
sudo nano /etc/systemd/system/semaphore.service
```

  
In that file, paste the following:  

```
[Unit]

Description=Semaphore Ansible UI

Documentation=https://github.com/ansible-semaphore/semaphore

Wants=network-online.target

After=network-online.target

[Service]

Type=simple

ExecReload=/bin/kill -HUP $MAINPID

ExecStart=/usr/bin/semaphore server --config /etc/semaphore/config.json

SyslogIdentifier=semaphore

Restart=always

[Install]

WantedBy=multi-user.target
```

  
Save and close the file. (CTL + O then CTL + X)

Reload the systemd daemon with:  

```
sudo systemctl daemon-reload
sudo systemctl start semaphore
```

  
Start and enable the Semaphore service with:  

```
sudo systemctl enable --now semaphore
```

## Accessing the Semaphore Web UI

With the service running and accepting connections, open a web browser that’s on a machine connected to the same LAN and point it to http://SERVER:3000 (Where SERVER is the IP address of the hosting server). You will be greeted by the Semaphore login prompt (Figure 1).
