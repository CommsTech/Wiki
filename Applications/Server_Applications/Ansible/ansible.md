---
dateModified:
title: Ansible
description: A guide to using Ansible, an open-source configuration management and automation tool. It may include instructions on how to install, configure, and use Ansible for managing IT infrastructure and deploying applications.
published: true
date: 2022-07-25T13:15:54.706Z
tags:
  - Ansible
  - Configuration_Management
  - IT_Infrastructure
editor: markdown
dateCreated: 2022-05-21T15:28:21.146Z
aliases:
---
# Ansible
Ansible is an open source community project sponsored by Red Hat, it's the simplest way to automate IT. Ansible is the only automation language that can be used across entire IT teams from systems and network administrators to developers and managers


## Ansible Cheat-Sheet
### Step 1 - Install Ansible on Ubuntu
PIP Method
1. Install PIP
```bash
sudo apt install python3-pip
```
2. Install Ansible
```bash
pip3 install ansible
```
3. Add execution path
```bash

```

APT Method

```
sudo apt-add-repository ppa:ansible/ansible
```

Press `ENTER` when prompted to accept the PPA addition.

Next, refresh your system’s package index so that it is aware of the packages available in the newly included PPA:

```
sudo apt update
```

Following this update, you can install the Ansible software with:

```
sudo apt install ansible
```

Your Ansible control node now has all of the software required to administer your hosts. Next, we will go over how to add your hosts to the control node’s inventory file so that it can control them.


### Step 2 — Setting Up the Inventory File

The _inventory file_ contains information about the hosts you’ll manage with Ansible. You can include anywhere from one to several hundred servers in your inventory file, and hosts can be organized into groups and subgroups. The inventory file is also often used to set variables that will be valid only for specific hosts or groups, in order to be used within playbooks and templates. Some variables can also affect the way a playbook is run, like the `ansible_python_interpreter` variable that we’ll see in a moment.

To edit the contents of your default Ansible inventory, open the `/etc/ansible/hosts` file using your text editor of choice, on your Ansible control node:

```
	sudo nano /etc/ansible/hosts
```

Copy

**Note**: Although Ansible typically creates a default inventory file at `etc/ansible/hosts`, you are free to create inventory files in any location that better suits your needs. In this case, you’ll need to provide the path to your custom inventory file with the `-i` parameter when running Ansible commands and playbooks. Using per-project inventory files is a good practice to minimize the risk of running a playbook on the wrong group of servers.

The default inventory file provided by the Ansible installation contains a number of examples that you can use as references for setting up your inventory. The following example defines a group named `[servers]` with three different servers in it, each identified by a custom alias: **server1**, **server2**, and **server3**. Be sure to replace the highlighted IPs with the IP addresses of your Ansible hosts.

/etc/ansible/hosts

```
[servers]
server1 ansible_host=203.0.113.111
server2 ansible_host=203.0.113.112
server3 ansible_host=203.0.113.113

[all:vars]
ansible_python_interpreter=/usr/bin/python3
```

The `all:vars` subgroup sets the `ansible_python_interpreter` host parameter that will be valid for all hosts included in this inventory. This parameter makes sure the remote server uses the `/usr/bin/python3` Python 3 executable instead of `/usr/bin/python` (Python 2.7), which is not present on recent Ubuntu versions.

When you’re finished, save and close the file by pressing `CTRL+X` then `Y` and `ENTER` to confirm your changes.

Whenever you want to check your inventory, you can run:

```
ansible-inventory --list -y
```

Copy

You’ll see output similar to this, but containing your own server infrastructure as defined in your inventory file:

```
Outputall:
  children:
    servers:
      hosts:
        server1:
          ansible_host: 203.0.113.111
          ansible_python_interpreter: /usr/bin/python3
        server2:
          ansible_host: 203.0.113.112
          ansible_python_interpreter: /usr/bin/python3
        server3:
          ansible_host: 203.0.113.113
          ansible_python_interpreter: /usr/bin/python3
    ungrouped: {}
```

Now that you’ve configured your inventory file, you have everything you need to test the connection to your Ansible hosts.

### Step 3 — Validate inventory and connections

After setting up the inventory file to include your servers, it’s time to check if Ansible is able to connect to these servers and run commands via SSH.

For this guide, we’ll be using the Ubuntu **root** account because that’s typically the only account available by default on newly created servers. If your Ansible hosts already have a regular sudo user created, you are encouraged to use that account instead.

You can use the `-u` argument to specify the remote system user. When not provided, Ansible will try to connect as your current system user on the control node.

From your local machine or Ansible control node, run:

```
ansible all -m ping -u root
```

Copy

This command will use Ansible’s built-in [`ping` module](https://docs.ansible.com/ansible/latest/modules/ping_module.html) to run a connectivity test on all nodes from your default inventory, connecting as **root**. The `ping` module will test:

- if hosts are accessible;
- if you have valid SSH credentials;
- if hosts are able to run Ansible modules using Python.

You should get output similar to this:

```
Outputserver1 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
server2 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
server3 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

If this is the first time you’re connecting to these servers via SSH, you’ll be asked to confirm the authenticity of the hosts you’re connecting to via Ansible. When prompted, type `yes` and then hit `ENTER` to confirm.

Once you get a `"pong"` reply back from a host, it means you’re ready to run Ansible commands and playbooks on that server.

**Note**: If you are unable to get a successful response back from your servers, check our [Ansible Cheat Sheet Guide](https://www.digitalocean.com/community/tutorials/how-to-use-ansible-cheat-sheet-guide) for more information on how to run Ansible commands with different connection options.


### Step 4 - SSH Key pair Setup (Optional)
run the following command

` ls -la .ssh `
```
commstech@clustermgr:~$ ls -la .ssh
total 40
drwx------  2 commstech commstech 4096 Mar  1 16:51 .
drwxr-xr-x 12 commstech commstech 4096 Feb 19 05:34 ..
-rw-------  1 commstech commstech 3389 Dec 22 10:46 ansible
-rw-r--r--  1 commstech commstech  746 Dec 22 10:46 ansible.pub
-rw-------  1 commstech commstech  746 Dec 22 11:44 authorized_keys
-rw-------  1 commstech commstech 8356 Dec 22 13:01 known_hosts
-rw-------  1 commstech commstech 7250 Dec 22 05:04 known_hosts.old
commstech@clustermgr:~$ 
```

Lets make some keys (easy)
` ssh-keygen -t ed25519 -C "Ansible_SSH_Default" `
-t = Keytype
-C = Comment / name

**note recommended not to put a passphrase**

```
commstech@clustermgr:~$ ssh-keygen -t ed25519 -C "Ansible_SSH_Default"
Generating public/private ed25519 key pair.
Enter file in which to save the key (/home/commstech/.ssh/id_ed25519): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/commstech/.ssh/id_ed25519
Your public key has been saved in /home/commstech/.ssh/id_ed25519.pub
The key fingerprint is:
SHA256:raUVdgfFatddK5tEGcWUzcD883+QPsy3PUPaw8tsl4w SSH_Default
The key's randomart image is:
+--[ED25519 256]--+
|            .*OXo|
|             B+o+|
|          o o..o+|
|         o ..o+o+|
|        S +..Eo.o|
|         =   o+=.|
|        o    +B.o|
|             EBX*|
|              .BX|
+----[SHA256]-----+
commstech@clustermgr:~$ 
```

now make sure its added

` ls -la .ssh `
```
commstech@clustermgr:~$ ls -la .ssh
total 48
drwx------  2 commstech commstech 4096 Mar  1 17:00 .
drwxr-xr-x 12 commstech commstech 4096 Feb 19 05:34 ..
-rw-------  1 commstech commstech 3389 Dec 22 10:46 ansible
-rw-r--r--  1 commstech commstech  746 Dec 22 10:46 ansible.pub
-rw-------  1 commstech commstech  746 Dec 22 11:44 authorized_keys
-rw-------  1 commstech commstech  444 Mar  1 17:00 id_ed25519
-rw-r--r--  1 commstech commstech   93 Mar  1 17:00 id_ed25519.pub
-rw-------  1 commstech commstech 8356 Dec 22 13:01 known_hosts
-rw-------  1 commstech commstech 7250 Dec 22 05:04 known_hosts.old
```

lets add this key to a server

` ssh-copy-id -i ~/.ssh/ansible.pub 192.168.255.7 `
so that's copy from /.ssh to the host at 192.168.255.7

```
commstech@clustermgr:~$ ssh-copy-id -i ~/.ssh/id_ed25519.pub 192.168.255.7
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/commstech/.ssh/id_ed25519.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
commstech@192.168.255.7's password: 

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh '192.168.255.7'"
and check to make sure that only the key(s) you wanted were added.

commstech@clustermgr:~$
```

lets test the key

` ssh -i ~/.ssh/ansible 192.168.255.7 `

Logged in perfectly !!!

### Step 5 - Lets do some installation and Ad-hoc commands

` sudo apt update `
` sudo apt install ansible -y `

** note I use nano so if you would like to follow along run ` sudo apt install nano -y `

` mkdir .ansible `
` cd .ansible `
` sudo nano inventory `
hosts being the name of my inventory file

add ips 1 per line (you can also use FQDN) mark categories with ` [] `

```
[ubuntu]
192.168.255.20
192.168.255.10
192.168.255.8
192.168.255.7
ubuntu.commsnet.org

[windows]
192.168.2.58
192.168.2.56
```

ok lets test that host file with the ping module (-m)

` ansible all --key-file ~/.ssh/id_ed25519 -i ~/.ansible/inventory/hosts -m ping `

```
~/.ansible/inventory$ ansible all --key-file ~/.ssh/ansible -i ~/.ansible/inventory/hosts -m ping
[WARNING]: Invalid characters were found in group names but not replaced, use -vvvv to see details
192.168.255.20 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
192.168.15.40 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
192.168.255.7 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
192.168.255.8 | UNREACHABLE! => {
    "changed": false,
    "msg": "Failed to connect to the host via ssh: ssh: connect to host 192.168.255.8 port 22: No route to host",
    "unreachable": true
}
192.168.15.2 | UNREACHABLE! => {
    "changed": false,
    "msg": "Failed to connect to the host via ssh: ssh: connect to host 192.168.15.2 port 22: Connection refused",
    "unreachable": true
}
192.168.15.3 | UNREACHABLE! => {
    "changed": false,
    "msg": "Failed to connect to the host via ssh: @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@\r\n@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @\r\n@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@\r\nIT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!\r\nSomeone could be eavesdropping on you right now (man-in-the-middle attack)!\r\nIt is also possible that a host key has just been changed.\r\nThe fingerprint for the ED25519 key sent by the remote host is\nSHA256:CkfK9sIOoQrTp/JU6+0lzKpSLtT45sautX3vDUcEkuw.\r\nPlease contact your system administrator.\r\nAdd correct host key in /home/commstech/.ssh/known_hosts to get rid of this message.\r\nOffending ECDSA key in /home/commstech/.ssh/known_hosts:16\r\n  remove with:\r\n  ssh-keygen -f \"/home/commstech/.ssh/known_hosts\" -R \"192.168.15.3\"\r\nHost key for 192.168.15.3 has changed and you have requested strict checking.\r\nHost key verification failed.",
    "unreachable": true
}
192.168.15.4 | UNREACHABLE! => {
    "changed": false,
    "msg": "Failed to connect to the host via ssh: @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@\r\n@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @\r\n@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@\r\nIT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!\r\nSomeone could be eavesdropping on you right now (man-in-the-middle attack)!\r\nIt is also possible that a host key has just been changed.\r\nThe fingerprint for the ED25519 key sent by the remote host is\nSHA256:QrmVdQlCLoj0sS0o47Q+oPYa+oceP273CbHFu42cXkA.\r\nPlease contact your system administrator.\r\nAdd correct host key in /home/commstech/.ssh/known_hosts to get rid of this message.\r\nOffending ECDSA key in /home/commstech/.ssh/known_hosts:18\r\n  remove with:\r\n  ssh-keygen -f \"/home/commstech/.ssh/known_hosts\" -R \"192.168.15.4\"\r\nHost key for 192.168.15.4 has changed and you have requested strict checking.\r\nHost key verification failed.",
    "unreachable": true
}
192.168.15.53 | UNREACHABLE! => {
    "changed": false,
    "msg": "Failed to connect to the host via ssh: commstech@192.168.15.53: Permission denied (publickey,password).",
    "unreachable": true
}
192.168.255.10 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
```

look like we have some failures due to not sending the ssh key beforehand.


lets now create an ansible config file

` sudo nano ~/.ansible/ansible.cfg `

```
[default]
inventory = ~/.ansible/inventory/hosts
private_key_file = ~/.ssh/ansible
```

now lets look at that previous command now with the new config file

` ansible all -m ping `

lets run another command to ensure that our host file is properly linked

` ansible all  --list-hosts `

now that we are linked properly lets run the gather facts module to inventory these systems

` ansible all -m gather_facts `

that was a big output.... we can reduce the number of devices pulled by running 
` ansible all -m gather_facts --limit 192.168.255.7`

**Checkout https://watch.thekitty.zone/watch?v=FPU9_KDTa8A for more info

lets try and run some privilege escalated commands
` ansible all -m apt -a update_cache=true `

it failed,
maybe ill try
` ansible all -m apt -a update_cache=true --become --ask-become-pass `

now we can install a package on all our servers
how about vim
` ansible all -m apt -a name=vim-nox --become --ask-become-pass `

now lets install a terminal emulator with tabs
` ansible all -m apt -a name=tmux --become --ask-become-pass `

we can update a individual package with the following command replacing "snapd" with the package you want to update
`ansible all -m apt -a "name=snapd state=latest" --become --ask-become-pass `

 but we can update all the packages with the following command
`ansible all -m apt -a "upgrade=dist" --become --ask-become-pass`

### Step 6 - Writing our first playbook
`nano install_apache.yml`

```
---

- hosts: ubuntu
  become: true
  tasks:

  - name: install apache2 package
    apt:
      name: apache2
```

lets run the play
`ansible-playbook --ask-become-pass install_apache.yml`

or we could setup a maintenance playbook for our ubuntu group
`nano maintenance.yml`

```
---

- hosts: ubuntu[0]
  become: true
  tasks:

    - name: package update and upgrade
      apt:
        update_cache: yes
        upgrade: 'yes'

```
**note that hosts: ubuntu[0] ubuntu being the name of the group and 0 being the number to exclude (see chart below)

webservers[0]       # == cobweb
webservers[-1]      # == weber
webservers[0:2]     # == webservers[0],webservers[1]
                    # == cobweb,webbing
webservers[1:]      # == webbing,weber
webservers[:3]      # == cobweb,webbing,weber

run the maintenance play
`ansible-playbook --ask-become-pass maintenance.yml`

### Conditional statements "when"
Follow along with learn linux tv @ https://watch.thekitty.zone/watch?v=BF7vIk9no14




## Setting Up Ansible Credentials and Host Files [](https://simeononsecurity.ch/guides/automate-windows-patching-and-updates-with-ansible/#setting-up-ansible-credentials-and-host-files)

Before we dive into automating Windows updates, let’s first set up the necessary credentials and host files in Ansible.

1. **Installing Ansible**: If you haven’t already, start by installing Ansible on your linux based ansible controller. You can follow the official Ansible documentation for detailed installation instructions: [Ansible Installation](https://docs.ansible.com/ansible/latest/installation_guide/index.html)
    
2. **Configuring Ansible Credentials**: To automate updates on Windows systems, Ansible requires the appropriate credentials. Ensure that you have the necessary administrative credentials for each target system. You can store these credentials securely using Ansible’s Vault or a password manager of your choice.
    
3. **Creating the Ansible Hosts File**: The Ansible hosts file defines the inventory of systems you want to manage. Create a text file called `hosts` and specify the target systems using their IP addresses or hostnames. For example:
    

```ini
[windows]
192.168.1.101
192.168.1.102
```

Copy

4. **Defining Ansible Variables**: To make the automation process more flexible, you can define variables in Ansible. For Windows updates, you might want to specify the desired update schedule or any additional configurations. Variables can be defined in the `hosts` file or separate variable files.

---

## Automating Windows Updates Using Ansible [](https://simeononsecurity.ch/guides/automate-windows-patching-and-updates-with-ansible/#automating-windows-updates-using-ansible)

With the basic setup in place, let’s now explore how to automate Windows updates using Ansible.

See the role and files:

- [Ansible Galaxy - windows_update](https://galaxy.ansible.com/simeononsecurity/windows_update)
- [Github - simeononsecurity/ansible_windows_update](https://github.com/simeononsecurity/ansible_windows_update)

1. **Creating the Ansible Playbook**: Ansible playbooks are YAML files that define a series of tasks to be executed on target systems. Create a new YAML file called `update_windows.yml` and define the necessary tasks.

```yaml
---
- name: Install Security Patches for Windows
  hosts: windows
  gather_facts: false

  tasks:
    - name: Check for available updates
      win_updates:
        category_names:
          - SecurityUpdates
        state: searched
      register: win_updates_result

    - name: Install security updates
      win_updates:
        category_names:
          - SecurityUpdates
        state: installed
      when: win_updates_result.updates | length > 0
```

Copy

Save it in a file named install_security_patches.yml

This playbook first checks for available security updates using the `win_updates` module with the `SecurityUpdates` category. The result is registered in the `win_updates_result` variable. Then, the playbook proceeds to install the security updates if there are any available.

2. **Using Ansible Modules**: Ansible provides various modules to interact with Windows systems. The `win_updates` module is specifically designed for managing Windows updates. Within your playbook, use this module to install updates, check for available updates, or reboot systems if required. Refer to the official Ansible documentation for detailed information on using the `win_updates` module: [Ansible Windows Modules](https://docs.ansible.com/ansible/latest/collections/ansible/windows/win_updates_module.html)
    
3. **Running the Ansible Playbook**: Once you have defined the tasks in your playbook, run it using the `ansible-playbook` command, specifying the playbook file and the target hosts. For example:
    

```shell
ansible-playbook -i hosts install_security_patches.yml
```

Copy

4. **Schedule Regular Execution**: To ensure that updates are applied consistently, you can schedule the execution of the Ansible playbook at regular intervals. Tools like cron (on Linux) or Task Scheduler (on Windows) can be used to automate this process. You should use cron to for this specifically as the playbook is launched from an linux based ansible controller.

Open crontab

```bash
   crontab -e
```

Copy

Add the following line after you modify it

```text
0 3 * * * ansible-playbook -i /path/to/hosts /path/to/playbook.yml
```

Copy


# Attempt 2 to learn Ansible 
Install ansible... see previous notes
step 2 (following along with https://invidious.commsnet.org/watch?v=NFDNPqSy2ZQ)

```
ansible --version
```

the output will look similar to this

```
commstech@clustermgr:~$ ansible --version
ansible [core 2.15.2]
  config file = None
  configured module search path = ['/home/commstech/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3/dist-packages/ansible
  ansible collection location = /home/commstech/.ansible/collections:/usr/share/ansible/collections
  executable location = /usr/bin/ansible
  python version = 3.10.12 (main, Jun 11 2023, 05:26:28) [GCC 11.4.0] (/usr/bin/python3)
  jinja version = 3.0.3
  libyaml = True
commstech@clustermgr:~$ 
```


cd into /etc/ansible

touch /etc/ansible.cfg

nano /etc/ansible.cfg
```
[defaults]
inventory=/etc/ansible/hosts
private_key_file = ~/.ssh/ansible
```
touch /etc/hosts

nano /etc/hosts

```
commstech@clustermgr:/etc/ansible$ sudo nano hosts            
  GNU nano 6.2                                                                                        hosts                                                                                                 
[ubuntu]
RemoteSupport ansible_host=192.168.255.20
192.168.255.15
ClusterMGR ansible_host=192.168.255.10
Nextcloud ansible_host=192.168.255.7
PXEServer ansible_host=192.168.15.40
Fireball_ubuntu ansible_host=192.168.4.110
PeachTea_ubuntu ansible_host=192.168.5.110
#
[truenas]
192.168.15.2
192.168.15.3
192.168.15.4
192.168.15.50
192.168.15.53
#
[ios]
Home_Switch ansible_host=192.168.15.60
[ios:vars]
ansible_ssh_pass=cisco
ansible_user=cisco
ansible_network_os=ios
ansible_connection=network_cli
ansible_password=cisco
auth_pass=cisco
#
[macs]
192.168.2.77
#
[pfsense]
192.168.15.126
192.168.4.126
192.168.5.126
#
[pis]
192.168.255.21
192.168.255.22
192.168.255.23
192.168.255.24
#
[windows]
192.168.2.58
192.168.2.56























commstech@clustermgr:/etc/ansible$ ansible -m ping Home_Switch
[WARNING]: ansible-pylibssh not installed, falling back to paramiko
Home_Switch | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
commstech@clustermgr:/etc/ansible$ ansible -m ping PXEServer
PXEServer | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
commstech@clustermgr:/etc/ansible$ ansible -m ping ios
[WARNING]: ansible-pylibssh not installed, falling back to paramiko
Home_Switch | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
commstech@clustermgr:/etc/ansible$ ansible -m ping ubuntu
PXEServer | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
ClusterMGR | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
RemoteSupport | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
192.168.255.15 | UNREACHABLE! => {
    "changed": false,
    "msg": "Failed to connect to the host via ssh: ssh: connect to host 192.168.255.15 port 22: No route to host",
    "unreachable": true
}
Nextcloud | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
Fireball_ubuntu | UNREACHABLE! => {
    "changed": false,
    "msg": "Failed to connect to the host via ssh: ssh: connect to host 192.168.4.110 port 22: Connection timed out",
    "unreachable": true
}
PeachTea_ubuntu | UNREACHABLE! => {
    "changed": false,
    "msg": "Failed to connect to the host via ssh: ssh: connect to host 192.168.5.110 port 22: Connection timed out",
    "unreachable": true
}

```

```
commstech@clustermgr:/etc/ansible$ ansible --version
ansible [core 2.15.2]
  config file = /etc/ansible/ansible.cfg
  configured module search path = ['/home/commstech/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3/dist-packages/ansible
  ansible collection location = /home/commstech/.ansible/collections:/usr/share/ansible/collections
  executable location = /usr/bin/ansible
  python version = 3.10.12 (main, Jun 11 2023, 05:26:28) [GCC 11.4.0] (/usr/bin/python3)
  jinja version = 3.0.3
  libyaml = True
```


Video 3
mkdir /etc/ansible/playbooks

cd /etc/ansible/playbooks

sudo nano gather_facts.yml

```
sudo nano gather_facts.yml
[sudo] password for commstech: 
  GNU nano 6.2                                                                                  gather_facts.yml                                                                                            
---
  - name: Gather Some Facts
    hosts: ios               
    gather_facts: no

    tasks:
    - name: Gather IOS Facts
      cisco.ios.ios_facts:

    - name: View Facts
      debug:
        var: ansible_facts
```


ctl + O

ctl +X

run the play with
```
ansible-playbook gather_facts.yml
```

note if your output looks like this
```
commstech@clustermgr:/etc/ansible/playbooks$ ansible-playbook gather_facts.yml 

PLAY [Gather Some Facts] ***********************************************************************************************************************************************************************************

TASK [Gather IOS Facts] ************************************************************************************************************************************************************************************
[WARNING]: ansible-pylibssh not installed, falling back to paramiko
fatal: [Home_Switch]: FAILED! => {"changed": false, "msg": "[Errno None] Unable to connect to port 22 on 192.168.15.60"}

PLAY RECAP *************************************************************************************************************************************************************************************************
Home_Switch                : ok=0    changed=0    unreachable=0    failed=1    skipped=0    rescued=0    ignored=0   
```

youll need to run
```
pip install --user ansible-pylibssh
```

if it looks like this 

```
commstech@clustermgr:/etc/ansible/playbooks$ pip install --user ansible-pylibssh
Collecting ansible-pylibssh
  Downloading ansible_pylibssh-1.1.0-cp310-cp310-manylinux_2_24_x86_64.whl (2.3 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 2.3/2.3 MB 8.4 MB/s eta 0:00:00
Installing collected packages: ansible-pylibssh
Successfully installed ansible-pylibssh-1.1.0
commstech@clustermgr:/etc/ansible/playbooks$ ansible-playbook gather_facts.yml  

PLAY [Gather Some Facts] ***********************************************************************************************************************************************************************************

TASK [Gather IOS Facts] ************************************************************************************************************************************************************************************
fatal: [Home_Switch]: FAILED! => {"changed": false, "msg": "ssh connection failed: ssh connect failed: Connection refused"}

PLAY RECAP *************************************************************************************************************************************************************************************************
Home_Switch                : ok=0    changed=0    unreachable=0    failed=1    skipped=0    rescued=0    ignored=0   
```

youll need to check your switch SSH ACL and ensure that your ansible server is allowed

```
line vty 0 4
 session-timeout 10 
 access-class 10 in
 exec-timeout 9 59
 logging synchronous
 vacant-message ^CTerminal Disconnected^C
 transport preferred ssh
 transport input ssh
 transport output none
line vty 5 15
 session-timeout 10 
 access-class 10 in
 exec-timeout 9 59
 logging synchronous
 vacant-message ^CTerminal Disconnected^C
 transport preferred ssh
 transport input ssh
 transport output none
Home_Switch#sh access
Home_Switch#sh access-li
Home_Switch#sh access-lists 10
Standard IP access list 10
    30 permit 192.168.255.25 log (201978 matches)
    40 permit 192.168.255.121
    10 permit 192.168.2.0, wildcard bits 0.0.0.127 (42 matches)
    20 permit 192.168.15.0, wildcard bits 0.0.0.127 (26 matches)
Home_Switch#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Home_Switch(config)#access
Home_Switch(config)#access-li
Home_Switch(config)#access-list 10
% Incomplete command.

Home_Switch(config)#access-list 10 ?
  <1-2147483647>  Sequence Number
  deny            Specify packets to reject
  permit          Specify packets to forward
  remark          Access list entry comment

Home_Switch(config)#access-list 10 25 per
Home_Switch(config)#access-list 10 25 permit 192.168.255.10 ?
  A.B.C.D  Wildcard bits
  log      Log matches against this entry
  <cr>     <cr>

Home_Switch(config)#access-list 10 25 permit 192.168.255.10 log
Home_Switch(config)#exit
Home_Switch#wr mem
```


a successful connection will result in some facts like the following

```
commstech@clustermgr:/etc/ansible/playbooks$ ansible-playbook gather_facts.yml 

PLAY [Gather Some Facts] ***********************************************************************************************************************************************************************************

TASK [Gather IOS Facts] ************************************************************************************************************************************************************************************
ok: [Home_Switch]

TASK [View Facts] ******************************************************************************************************************************************************************************************
ok: [Home_Switch] => {
    "ansible_facts": {
        "net_api": "cliconf",
        "net_gather_network_resources": [],
        "net_gather_subset": [
            "default"
        ],
        "net_hostname": "Home_Switch",
        "net_image": "flash:packages.conf",
        "net_iostype": "IOS-XE",
        "net_model": "WS-C3850-48P",
        "net_operatingmode": "controller",
        "net_python_version": "3.10.12",
        "net_serialnum": "FOC2XXXXXXXXXX",
        "net_stacked_models": [
            "WS-C3850-48P"
        ],
        "net_stacked_serialnums": [
            "FOC2XXXXXXXXXXXXX"
        ],
        "net_system": "ios",
        "net_version": "16.12.09",
        "net_virtual_switch": "STACK",
        "network_resources": {}
    }
}
```

you can set conditions based by the following

```
commstech@clustermgr:/etc/ansible/playbooks$ sudo nano gather_facts.yml        
  GNU nano 6.2                                                                                  gather_facts.yml                                                                                            
---
  - name: Gather Some Facts
    hosts: ios  
    gather_facts: no

    tasks:
    - name: Gather IOS Facts
      cisco.ios.ios_facts:
      when: ansible_network_os == 'ios'

    - name: View Facts
      debug:
        var: ansible_facts
```









## Adding Windows Clients
on the windows computer add an ansible user
set a good password
add the ansible user to the administrator group

**note windows Home does not have local users and groups in computer management... youll have to run the following command to add a local admin. win + R

```
netplwiz
```

open a powershell prompt as admin

Validate if you have both ssh client and server
```
Get-WindowsCapability -Online | Where-Object Name -like 'OpenSSH*' 
```

install the missing application with

```
Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0
Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0

Start-Service sshd
Set-Service -Name sshd -StartupType 'Automatic'

```

add some firewall rules in windows

```

```

Modify your ansible hosts file for the windows client and ansible user

```
[win]
IP_ADDRESS

[win:vars]
ansible_user=Administrator
ansible_password=
ansible_connection=ssh
ansible_shell_type=cmd
ansible_ssh_common_args=-o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null
ansible_ssh_retries=3 ansible_become_method=runas
```


validate you can ssh into the windows server
```
commstech@clustermgr:/etc/ansible$ ssh ansible@192.168.2.56
ansible@192.168.2.56's password: 
```

should look like this if you were successful
```
Microsoft Windows [Version 10.0.22621.2134]
(c) Microsoft Corporation. All rights reserved.

ansible@TRAVELBOOK C:\Users\ansible>
```


---

## Conclusion [](https://simeononsecurity.ch/guides/automate-windows-patching-and-updates-with-ansible/#conclusion)

Automating Windows updates with Ansible can greatly simplify the management of updates across your infrastructure. By following the steps outlined in this article, you can set up Ansible credentials, define host files, and create playbooks to automate the update process. Embracing automation not only saves time but also ensures that your Windows systems are up to date, secure, and operating at their best.

Remember to stay informed about relevant government regulations such as the [NIST Cybersecurity Framework](https://www.nist.gov/cyberframework) or [ISO/IEC 27001](https://www.iso.org/isoiec-27001-information-security.html) , which provide guidelines and best practices for maintaining a secure and compliant environment.

---

## References [](https://simeononsecurity.ch/guides/automate-windows-patching-and-updates-with-ansible/#references)

- [Ansible Documentation](https://docs.ansible.com/ansible/latest/index.html)
- [Ansible Installation Guide](https://docs.ansible.com/ansible/latest/installation_guide/index.html)
- [Ansible Windows Modules](https://docs.ansible.com/ansible/latest/collections/ansible/windows/win_updates_module.html)
- [Ansible Galaxy - windows_update](https://galaxy.ansible.com/simeononsecurity/windows_update)
- [Github - simeononsecurity/ansible_windows_update](https://github.com/simeononsecurity/ansible_windows_update)
- [NIST Cybersecurity Framework](https://www.nist.gov/cyberframework)
- [ISO/IEC 27001 - Information Security](https://www.iso.org/isoiec-27001-information-security.html)
- [automate windows patching and updates with ansible](https://simeononsecurity.ch/guides/automate-windows-patching-and-updates-with-ansible/)