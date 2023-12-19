# How to Manage Remote Windows Host using Ansible

Last Updated: August 17, 2021 by [James Kiarie](https://www.linuxtechi.com/author/james/ "View all posts by James Kiarie")

**Ansible** is increasingly becoming the go-to platform for application deployment, and software provisioning among developers owing to its ease of use and flexibility. Furthermore, it is easy to set up and no agent is required to be installed on remote nodes, instead, Ansible uses password less SSH authentication to manage remote Unix/Linux hosts. In this topic, however, we are going to see how you can manage Windows Host using Ansible.

[![Manage-Windows-Hosts-using-Ansible](https://www.linuxtechi.com/wp-content/uploads/2019/11/Manage-Windows-Hosts-using-Ansible.jpg?ezimgfmt=ng%3Awebp%2Fngcb22%2Frs%3Adevice%2Frscb22-1 "Manage Windows Hosts using Ansible")](https://www.linuxtechi.com/wp-content/uploads/2019/11/Manage-Windows-Hosts-using-Ansible.jpg)

**Lab setup**

We shall use the setup below to accomplish our objective

- Ansible Control node   –    CentOS 8          –     IP: 192.168.43.13
- Windows 10 node         –    Windows 10     –     IP: 192.168.43.147

Table of Contents

[](https://www.linuxtechi.com/manage-windows-host-using-ansible/#)

- [Part 1 Installing Ansible on the Control node (CentOS 8)](https://www.linuxtechi.com/manage-windows-host-using-ansible/#Part_1_Installing_Ansible_on_the_Control_node_CentOS_8 "Part 1 Installing Ansible on the Control node (CentOS 8)")
    - [Step 1 Verify that Python3 is installed on Ansible control node](https://www.linuxtechi.com/manage-windows-host-using-ansible/#Step_1_Verify_that_Python3_is_installed_on_Ansible_control_node "Step 1 Verify that Python3 is installed on Ansible control node")
    - [Step 2 Install a virtual environment for running Ansible](https://www.linuxtechi.com/manage-windows-host-using-ansible/#Step_2_Install_a_virtual_environment_for_running_Ansible "Step 2 Install a virtual environment for running Ansible")
    - [Step 3 Install Ansible](https://www.linuxtechi.com/manage-windows-host-using-ansible/#Step_3_Install_Ansible "Step 3 Install Ansible")
    - [Step 4 Install Pywinrm](https://www.linuxtechi.com/manage-windows-host-using-ansible/#Step_4_Install_Pywinrm "Step 4 Install Pywinrm")
- [Part 2 Configuring Windows Host](https://www.linuxtechi.com/manage-windows-host-using-ansible/#Part_2_Configuring_Windows_Host "Part 2 Configuring Windows Host")
    - [Step 1 Download the WinRM script on Windows 10 host](https://www.linuxtechi.com/manage-windows-host-using-ansible/#Step_1_Download_the_WinRM_script_on_Windows_10_host "Step 1 Download the WinRM script on Windows 10 host")
    - [Step 2 Run the WinRM script on Windows 10 host](https://www.linuxtechi.com/manage-windows-host-using-ansible/#Step_2_Run_the_WinRM_script_on_Windows_10_host "Step 2 Run the WinRM script on Windows 10 host")
- [Part 3 Connecting to Windows Host from Ansible Control Node](https://www.linuxtechi.com/manage-windows-host-using-ansible/#Part_3_Connecting_to_Windows_Host_from_Ansible_Control_Node "Part 3 Connecting to Windows Host from Ansible Control Node")
- [Part 4 Creating and running a playbook for Windows 10 host](https://www.linuxtechi.com/manage-windows-host-using-ansible/#Part_4_Creating_and_running_a_playbook_for_Windows_10_host "Part 4 Creating and running a playbook for Windows 10 host")

### Part 1: Installing Ansible on the Control node (CentOS 8)

Before anything else, we need to get Ansible installed on the Control node which is the CentOS 8 system.

#### Step 1: Verify that Python3 is installed on Ansible control node

Firstly, we need to confirm if Python3 is installed. CentOS 8 ships with Python3 but if it’s missing for any reason, install using the command:

# sudo dnf install python3

Next, make Python3 the default Python version by running:

# sudo alternatives --set python /usr/bin/python3

To verify if python3 is installed, run the command:

# python --version

 **![check-python-version](https://www.linuxtechi.com/wp-content/uploads/2019/11/check-python-version.png?ezimgfmt=rs:604x89/rscb22/ng:webp/ngcb22 "check python version")**

**Read Also :** **[How to Install Ansible (Automation Tool) on CentOS 8/RHEL 8](https://www.linuxtechi.com/install-ansible-centos-8-rhel-8/)**

#### Step 2: Install a virtual environment for running Ansible

For this exercise, an isolated environment for running and testing Ansible is preferred. This will keep at bay issues such as dependency problems and package conflicts. The isolated environment we are going to create is called a virtual environment.

Firstly, let’s begin with the installation of the virtual environment on CentOS 8.

# sudo dnf install python3-virtualenv

![install-python3-virtualenv](https://www.linuxtechi.com/wp-content/uploads/2019/11/install-python3-virtualenv.png?ezimgfmt=rs:640x358/rscb22/ng:webp/ngcb22 "install python3 virtualenv")

After the installation of the virtual environment, create a virtual workspace by running:

# virtualenv env

![virtualenv-env-ansible](https://www.linuxtechi.com/wp-content/uploads/2019/11/virtualenv-env-ansible.png?ezimgfmt=rs:636x96/rscb22/ng:webp/ngcb22 "virtualenv env ansible")

# source env/bin/activate

![source-env-bin-activate-ansible](https://www.linuxtechi.com/wp-content/uploads/2019/11/source-env-bin-activate-ansible.png?ezimgfmt=rs:628x77/rscb22/ng:webp/ngcb22 "source env bin activate ansible")

Great! Observer that the prompt has now changed to (env).

#### Step 3: Install Ansible

After the creation of the virtual environment, proceed and install Ansible automation tool using pip as shown:

# pip install ansible

![pip-install-Ansible](https://www.linuxtechi.com/wp-content/uploads/2019/11/pip-install-Ansible.png?ezimgfmt=rs:639x299/rscb22/ng:webp/ngcb22 "pip install Ansible")

You can later confirm the installation of Ansible using the command:

# ansible --version

![check-ansible-version](https://www.linuxtechi.com/wp-content/uploads/2019/11/check-ansible-version.png?ezimgfmt=rs:635x164/rscb22/ng:webp/ngcb22 "check ansible version")

To test Ansible and see if it’s working on our Ansible Control server run:

# ansible localhost -m ping

![Test-ansible-for-connectivity](https://www.linuxtechi.com/wp-content/uploads/2019/11/Test-ansible-for-connectivity.png?ezimgfmt=rs:631x110/rscb22/ng:webp/ngcb22 "Test ansible for connectivity")

Great! Next, we need to define the Windows host or system on a host file on the Ansible control node. Therefore, open the default hosts file

# vim /etc/ansible/hosts

Define the Windows hosts as shown below.

![Ansible-hosts-file](https://www.linuxtechi.com/wp-content/uploads/2019/11/Ansible-hosts-file.png?ezimgfmt=rs:636x256/rscb22/ng:webp/ngcb22 "Ansible hosts file")

**Note:** The username and password point to the user on the Windows host system.

Next, save and exit the configuration file.

#### Step 4: Install Pywinrm

Unlike in Unix systems where Ansible uses SSH to communicate with remote hosts, with Windows it’s a different story altogether. To communicate with Windows hosts, you need to install Winrm.

To install winrm, once again, use pip tool as shown:

# pip install pywinrm

![install-pywinrm](https://www.linuxtechi.com/wp-content/uploads/2019/11/install-pywinrm.png?ezimgfmt=rs:639x345/rscb22/ng:webp/ngcb22 "install pywinrm")

### Part 2: Configuring Windows Host

In this section, we are going to configure our Windows 10 remote host system to connect with the Ansible Control node. We are going to install the **WinRM listener-** short for **Windows Remote** – which will allow the connection between the Windows host system and the Ansible server.

But before we do so, your Windows host system needs to fulfill a few requirements for the installation to succeed:

- Your Windows host system should be **Windows 7 or later**. For Servers, ensure that you are using **Windows Server 2008** and later versions.
- Ensure your system is running **.NET Framework 4.0** and later.
- Windows **PowerShell** should be Version 3.0 & later

With all the requirements met, now follow the steps stipulated below:

#### Step 1: Download the WinRM script on Windows 10 host

WinRM can be installed using a script that you can download from this [link](https://raw.githubusercontent.com/ansible/ansible/devel/examples/scripts/ConfigureRemotingForAnsible.ps1). Copy the entire script and paste it onto the notepad editor. Thereafter, ensure you save the WinRM script at the most convenient location. In our case, we have saved the file on the Desktop under the name  ConfigureRemotingForAnsible.ps1

#### Step 2: Run the WinRM script on Windows 10 host

Next, run PowerShell as the Administrator

![Run-PowerShell-as-Administrator](https://www.linuxtechi.com/wp-content/uploads/2019/11/Run-PowerShell-as-Administrator.png?ezimgfmt=rs:529x262/rscb22/ng:webp/ngcb22 "Run PowerShell as Administrator")

Navigate to the script location and run it. In this case, we have navigated to the Desktop location where we saved the script. Next, proceed and execute the WinRM script on the WIndows host:

.\ConfigureRemotingForAnsible.ps1

This takes about a minute and you should get the output shown below. The output shows that WinRM has successfully been installed.

![set-up-WinRM-on-Windows10](https://www.linuxtechi.com/wp-content/uploads/2019/11/set-up-WinRM-on-Windows10.png?ezimgfmt=rs:654x215/rscb22/ng:webp/ngcb22 "set up WinRM on Windows10")

### Part 3: Connecting to Windows Host from Ansible Control Node

To test connectivity to the Windows 10 host, run the command:

# ansible winhost -m win_ping

![Ansible-ping-windows-host-machine](https://www.linuxtechi.com/wp-content/uploads/2019/11/Ansible-ping-windows-host-machine.png?ezimgfmt=rs:652x100/rscb22/ng:webp/ngcb22 "Ansible ping windows host machine")

The output shows that we have indeed established a connection to the remote Windows 10 host from the Ansible Control node. This implies that we can now manage the remote Windows host using Ansible Playbooks. Let’s create a sample playbook for the Windows host system.

### Part 4: Creating and running a playbook for Windows 10 host

In this final section, we shall create a playbook and create a task that will install Chocolatey on the remote host. Chocolatey is a package manager for Windows system. The play is defined as shown:

# vim chocolatey.yml
---
- hosts: winhost
  gather_facts: no
  tasks:
   - name: Install Chocolatey on Windows10
     win_chocolatey: name=procexp  state=present

![Ansible-Playbook-install-chocolatey](https://www.linuxtechi.com/wp-content/uploads/2019/11/Ansible-Playbook-install-chocolatey.png?ezimgfmt=rs:631x183/rscb22/ng:webp/ngcb22 "Ansible Playbook install chocolatey")

Save and close the yml file. Next, execute the playbook as shown

# ansible-playbook chocolatey.yml

![Ansible-playBook-succeeded](https://www.linuxtechi.com/wp-content/uploads/2019/11/Ansible-playBook-succeeded-1024x225.png?ezimgfmt=rs:634x139/rscb22/ng:webp/ngcb22 "Ansible playBook succeeded")

The output is a pointer that all went well. And this concludes this topic on how you can manage Windows host using Ansible.