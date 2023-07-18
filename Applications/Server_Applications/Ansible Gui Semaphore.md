[Red Hat](https://www.openshift.com/try?utm_content=inline-mention)‘s open source [Ansible](https://www.ansible.com/) is an open source IT automation platform, [written in Python](https://thenewstack.io/an-introduction-to-python-a-language-for-the-ages/), that can configure systems, deploy software, and orchestrate advanced workflows. By default, Ansible is a command-line tool but isn’t terribly complicated to work with.

However, there are some who’d much prefer having a graphical user interface (GUI) to make the platform more efficient to use. Thankfully, there’s one particular GUI, called Semaphore, that can help make using Ansible easier for larger environments and organizations.

I want to walk you through the process of installing [Semaphore](https://www.ansible-semaphore.com/). I’m going to demonstrate on Ubuntu Linux (version 22.04), so you’ll want to make sure to have Ansible installed and working. To do that, make sure to first follow [this tutorial](https://thenewstack.io/install-ansible-on-ubuntu-server-to-automate-linux-server-deployments/). Once you’ve taken care of that, you are ready to install Semaphore.

## What You’ll Need

Obviously, you’ll need Ansible up and running on Ubuntu. You’ll also need a user with `sudo` privileges. That’s it. Let’s get to the installation.

## Installing Semaphore

Although you can easily install Semaphore with Snap, we’re going to go a different route, so we can ensure the platform is available from anywhere on your LAN.

The first thing to do is install a database server.  We’re going to go with MariaDB. To install MariaDB on Ubuntu, you must add the repository with the command:  

|   |   |
|---|---|
|1|curl -LsS https://downloads.mariadb.com/MariaDB/mariadb_repo_setup \| sudo bash -s —|

  
After that command finishes, install both the server and client with:  

|   |   |
|---|---|
|1|sudo apt install mariadb-server mariadb-client|

  
With MariaDB installed, secure it with the command:  

|   |   |
|---|---|
|1|sudo mariadb-secure-installation|

  
Answer n to the first question and y to the remaining. You’ll also be prompted to create and verify a root user password.

With the database installed, it’s time to add Semaphore. We’ll first set a variable for the version with the command:  

|   |   |
|---|---|
|1|VER=$(curl -s https://api.github.com/repos/ansible-semaphore/semaphore/releases/latest\|grep tag_name \| cut -d '"' -f 4\|sed 's/v//g')|

  
We can now use that variable to download the correct version with the command:  

|   |   |
|---|---|
|1|wget https://github.com/ansible-semaphore/semaphore/releases/download/v${VER}/semaphore_${VER}_linux_amd64.deb|

  
Install Semaphore with:  

|   |   |
|---|---|
|1|sudo apt install ./semaphore_${VER}_linux_amd64.deb|

  
Boom! Semaphore is installed and ready to be configured.

## Configure Semaphore

You don’t just edit a configuration file because none exists yet. To generate the configure file, run semaphore such that it will prompt you to configure everything. The command for this is:  

|   |   |
|---|---|
|1|sudo semaphore setup|

  
The first section of the configuration looks like this:  

|   |   |
|---|---|
|1<br><br>2<br><br>3<br><br>4<br><br>5<br><br>6<br><br>7<br><br>8<br><br>9<br><br>10<br><br>11<br><br>12<br><br>13|Hello! You will now be guided through a setup to:<br><br>1. Set up configuration for a MySQL/MariaDB database<br><br>2. Set up a path for your playbooks (auto-created)<br><br>3. Run database Migrations<br><br>4. Set up initial semaphore user &amp; password<br><br>What database to use:<br><br>1 - MySQL<br><br>2 - BoltDB<br><br>3 - PostgreSQL<br><br>(default 1):|

  
Make sure to select MySQL for your database and then configure it accordingly. You can accept the default for everything, but you will have to type the MariaDB root user password you created earlier.

When you get to the Hostname section (which looks like db Hostname (default 127.0.0.1:3306):), make sure to type it in the form:  

|   |   |
|---|---|
|1|http://SERVER:3000|

  
Where SERVER is the IP address of your hosting server.

Near the end of the prompt, you’ll also be asked to create a new admin user for the web UI.

## Create a Systemd File

Next, we need to create a systemd file so the Semaphore service can be controlled. Create the file with the command:  

|   |   |
|---|---|
|1|sudo nano /etc/systemd/system/semaphore.service|

  
In that file, paste the following:  

|   |   |
|---|---|
|1<br><br>2<br><br>3<br><br>4<br><br>5<br><br>6<br><br>7<br><br>8<br><br>9<br><br>10<br><br>11<br><br>12<br><br>13<br><br>14<br><br>15|[Unit]<br><br>Description=Semaphore Ansible UI<br><br>Documentation=https://github.com/ansible-semaphore/semaphore<br><br>Wants=network-online.target<br><br>After=network-online.target<br><br>[Service]<br><br>Type=simple<br><br>ExecReload=/bin/kill -HUP $MAINPID<br><br>ExecStart=/usr/bin/semaphore server --config /etc/semaphore/config.json<br><br>SyslogIdentifier=semaphore<br><br>Restart=always<br><br>[Install]<br><br>WantedBy=multi-user.target|

  
Save and close the file.

Reload the systemd daemon with:  

|   |   |
|---|---|
|1|sudo systemctl daemon-reload|

  
Start and enable the Semaphore service with:  

|   |   |
|---|---|
|1|sudo systemctl enable --now semaphore|

## Accessing the Semaphore Web UI

With the service running and accepting connections, open a web browser that’s on a machine connected to the same LAN and point it to http://SERVER:3000 (Where SERVER is the IP address of the hosting server). You will be greeted by the Semaphore login prompt (Figure 1).