---
title: Ansible
description: 
published: true
date: 2022-07-25T13:15:54.706Z
tags: 
editor: markdown
dateCreated: 2022-05-21T15:28:21.146Z
---
# Ansible
Ansible is an open source community project sponsored by Red Hat, it's the simplest way to automate IT. Ansible is the only automation language that can be used across entire IT teams from systems and network administrators to developers and managers


## Ansible Cheat-Sheet
### Install Ansible on Ubuntu
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



## Ansible Setup
### SSH Setup
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
so thats copy from /.ssh to the host at 192.168.255.7

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

## Lets do some installation and Ad-hoc commands

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

### Writing our first playbook
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