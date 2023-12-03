
Detailed Technical Procedure (DTP)

for

Using Ansible to Configure Network Devices


Table of Contents

[Revision History 3](#_Toc148935373 "#_Toc148935373")

[1 Scope 4](#_Toc148935374 "#_Toc148935374")

[1.1 Objective 4](#_Toc148935375 "#_Toc148935375")

[1.2 Install Terminology and Requirements 4](#_Toc148935376 "#_Toc148935376")

[2 Technical Procedures 5](#_Toc148935377 "#_Toc148935377")

[2.1 Install Ansible 5](#_Toc148935378 "#_Toc148935378")

[2.2 Configure an Ansible Inventory 7](#_Toc148935379 "#_Toc148935379")

[2.3 Configuring a Jinja Configuration Template 12](#_Toc148935380 "#_Toc148935380")

[2.4 The Ansible Playbook 15](#_Toc148935381 "#_Toc148935381")

[3 Wrapping Up 19](#_Toc148935382 "#_Toc148935382")

[3.1 Limitations 19](#_Toc148935383 "#_Toc148935383")

[3.2 Next Steps 19](#_Toc148935384 "#_Toc148935384")

[3.3 Conclusion 20](#_Toc148935385 "#_Toc148935385")

 

# 1 Scope

## 1.1 Objective

This DTP is written to provide step-by-step instructions to install and operate Ansible to configure network devices. These instructions were written using Cisco devices, but most modern network OSes support Ansible through a CLI or SSH plugin. These instructions can be used with little or no modification for devices from non-Cisco vendors, though it is necessary to understand a vendor’s base CLI operations.

Ansible is a toolset developed by Michael DeHaan that is now owned and operated by the Red Hat software company. The toolset is an open-source automation suite that can be used for configuration management, provisioning systems, or orchestrating complex infrastructure deployments.

Refer to the Ansible online documentation for further details on use for a specific network OS or system.

## 1.2 Installation Terminology and Requirements

Ansible is operated from Linux distributions, but each distribution has some key differences when installing software packages. On Debian systems, the Advanced Package Tool (apt) is used for most package management. On RHEL-based systems, the Yellowdog Updater Modifier (yum) or Dandified YUM (dnf) are used instead. For the purposes of this document, package managers largely behave the same, so their specialized differences will be ignored.

When installing features on Linux, repositories are online databases of available software and the dependencies that must be installed to enable specific software. Some Linux software is included in an operating system’s base repositories from the start. However, different distributions have different base repos, and those repos may or may not include the software necessary for a specific task. Adding repos is required to enable many non-standard features on a Linux operating system. On Red Hat Enterprise Linux, different subscriptions may also be required depending on the version of RHEL being used. On RHEL, the system must be enabled in the subscription manager, the subscription for specific software must be enabled, and the correct repository needs to be added.

On Windows, there are not any officially supported binaries to run Ansible natively. Instead, Ansible can be run on Windows by emulating or virtualizing a Linux environment for the Ansible software suite. This can be achieved by using the Windows Subsystem for Linux or Cygwin.

 

# 2 Technical Procedures

## 2.1 Install Ansible

2.1.1 Ansible can be used on most operating systems with a minimal amount of pre-configuration. Installing the software packages requires privileged access to a command line interface (CLI) to install Ansible itself and a recent version of Python. For users that already have Ansible and Python, you may be able to skip this chapter. However, this DTP does include guidance on several best business practices that can be helpful for technicians that are less familiar with the software.

2.1.2 **Starting from scratch****:** Boot up and access a modern operating system that can support the installation of Ansible and Python. You will need a functional internet connection for these steps. Open a CLI terminal for your OS from an account that can run the administrative permissions required for installing software. For Linux systems, continue to the next step. For Windows systems, use the following sub-steps to install the Windows Subsystem for Linux with Ubuntu.

2.1.2.1 **WSL installation****.** Windows 10, 11, Server 2019 and Server 2022 can run Linux as a nested virtual environment that has built-in access to the file system. This document will only provide the base process, refer to online documentation for in-depth steps and assistance.

2.1.2.2 On Windows 10, 11, and Server 2022, use the command **wsl --install** from an elevated Command Prompt or PowerShell terminal. On Server 2019 1709 or higher, use the command **Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux** – on Server 2019, you also have to install the Linux Distribution as per this link: [https://learn.microsoft.com/en-us/windows/wsl/install-manual#downloading-distributions](#downloading-distributions "https://learn.microsoft.com/en-us/windows/wsl/install-manual#downloading-distributions")

2.1.2.3 Run the WSL distribution with the command **wsl** and configure a username and password if this is a first time install. This Linux account is separate from your Windows username and password.

2.1.2.4 By default, WSL has very open file permissions. Because of this, Ansible may error due to running from a world writable directory. Fix this by setting the umask for WSL on boot. With umask, we can set the default file permissions for the whole system. With a text editor like gedit, nano, or vim, use sudo to open the file /etc/wsl.conf e.g. **sudo vi /etc/wsl.conf** – this file may not exist yet, but it’s fine to create it as a new empty text document. Then, input the following lines:

_**[automount]**_

_**options=**__**"**__**metadata,umask=0033**__**"**_

2.1.2.5 Save the file and reboot your Windows machine. The next time WSL starts, it will use these settings to make Ansible operate as expected.

2.1.3 **Environment Setup and Dependencies.** We want to setup Python and its modules so that Ansible has a fully operation environment for network configuration management. To begin, update the Linux distro with **sudo** **apt update && apt upgrade -y** for Debian (Astra, Raspbian, Ubuntu) or **sudo** **dnf update -y** for RHEL-variants (CentOS, Fedora) while using **sudo yum update -y** on RHEL-actual. Notes: On Debian systems, use "apt update" to make sure that the system has an up-to-date list of available packages on the enabled repositories. On RHEL, register and enable the Red Hat subscription to pull updates ref: [https://www.redhat.com/sysadmin/rhel-subscription-manager](https://www.redhat.com/sysadmin/rhel-subscription-manager "https://www.redhat.com/sysadmin/rhel-subscription-manager")

2.1.3.1 Install packages required for Python by using the following commands. On Debian systems: **sudo apt install -y python3 python3-pip** and test the install with the python3 --version command. On RHEL-forked distros use **sudo dnf install -y python3 python3-pip** with the same verification. For RHEL-actual, on RHEL7 use **sudo subscription-manager-repos --enable rhel-7-server-optional-rpms --enable rhel-server-rhscl-7-rpms && yum install -y @development && yum install -y rh-python36 rh-python36-python-pip** – on RHEL8 use **sudo yum install -y python3 python3-pip** – on RHEL9 use **sudo dnf install -y python3 python3-pip**

2.1.3.2 Download Python packages that are necessary for basic network device configuration management. We’ll be using PipEnv to control our development environment, Paramiko to enable SSHv2 connections, and Jinja2 to build templates. These package dependencies can be managed by using Python-Pip; it is the package manager for Python projects. First, install Pipenv so the later packages will be added into a purpose-built environment for Ansible. This will prevent our Python packages from having version conflicts for other projects.

2.1.3.3 Use **pip install --user pipenv** to install the PipEnv package under your logged-in user profile. This prevents conflicts that can arise from installing a Python package for the system or root user. Note that we did not use "sudo" on this command. Make sure to run non-sudo commands without privilege elevation. Next, update your environment variables with the command **source ~/.profile** to make sure the newly installed package is accessible via the CLI. The ".profile" file should be stored in your user home directory and is usually only loaded upon login.

2.1.3.4 Make a project file directory for this guide. This will be the base directory used by Ansible and Python going forward. Use the command **mkdir ansible_for_networks && cd ./ansible_for_networks** to make and move into the new directory. Then install the paramiko and jinja2 packages in the project environment by typing **pipenv install paramiko jinja2**

2.1.3.5 Test the new virtualenv by using the **pipenv shell** command. The directory name should be prepended to the shell prompt e.g.

  (ansible_for_networks) user@hostname:/home/user/ansible_for_networks$

Use the command **exit** to leave the virtual environment shell.

2.1.4 Installing Ansible. On Debian distros: **sudo apt install -y software-properties-common &&** **sudo** **apt-add-repository --yes --update ppa:ansible/ansible** **&& sudo apt install -y ansible** to add the required install repository and then install Ansible itself. For a RHEL-forked distro, use **sudo dnf install -y epel-release && sudo dnf install -y ansible** to install the Ansible package. For RHEL-actual, on RHEL7 use **sudo subscription-manager repos --enable** **rhel-7-server-ansible-2.9-rpms** **&& sudo** **yum** **install** **-y** **ansible** – on RHEL8 use **sudo subscription-manager repos --enable ansible-2.9-for-rhel-8-x86_64-rpms && sudo yum install -y ansible** – on RHEL9 just **sudo dnf install -y ansible**

## 2.2 Configure an Ansible Inventory

2.2.1 **Setup the Ansible configuration file.** To perform reliably, Ansible should be configured with some default settings that meet each project’s requirements. By default, Ansible looks in several places on an OS in an attempt to automatically determine key aspects of your system. However, we can set a file in the local project directory that overrides any of that to ensure consistent functionality that aligns with our current project. With a text editor like nano or vim, create the file _ansible.cfg_. The following block of text includes commands to open and close vim after saving the file; pasting the entire block will not provide the "escape" character sequence. Comments are included to explain settings.

_**vi ansible.cfg**_

_**i**_

_**[defaults]**_

_**# Configure the inventory location where host details and variables will be found**_

_**inventory = ./inventory**_

_**# Set default output for running Ansible playbooks**_

_**interpreter_python = auto_silent**_

_**verbosity = 1**_

_**# Ignore new host key checking; to avoid doing this, update known_hosts properly**_

_**host_key_checking = False**_

_**# Disable 'requiretty' in /etc/sudoers to prevent conflicts caused by sudo operations**_

_**ANSIBLE_PIPELINING = True**_

_**# Set warnings and output configuration for per-host and per-variable responses by Ansible**_

_**retry_files_enabled = False**_

_**deprecation_warnings = False**_

_**system_warnings = True**_

_**command_warnings = False**_

_**error_on_undefined_vars = True**_

_**display_args_to_stdout = True**_

_**display_skipped_hosts = True**_

_**gather_subset = !hardware,!facter,!ohai**_

_**# Log the Ansible results somewhre**_

_**log_path = ansible_results.log**_

_**# Configure how SSH connections happen, primarily set timeouts and use SFTP for file transfers**_

_**[ssh_connection]**_

_**ssh_args = -C -o ControlMaster=no -o ControlPersist=60s -o ConnectTimeout=10**_

_**retries = 1**_

_**usetty = True**_

_**sftp_batch_mode = True**_

_**transfer_method = sftp**_

_**# Only make a change to configurations if there's a difference, prevents unneeded command signaling**_

_**[diff]**_

_**always = True**_

_**# TYPE ESCAPE - [ESC] key**_

_**# VIM command to write and quit**__**; the colon : indicates a command, w for write, q for quit**_

_**:wq**_

2.2.2 **Setting Up the File Structure.** Ansible tries to search through a default file structure for things like host information, variables, and templates. Again, it will look through various locations on the OS, but we’ll centralize all the files for our purposes in a very deliberate setup. This file structure can be represented as the following tree where "d" items are directories and "f" items are files with data in them.

--d /home/user/ansible_for_networks

--f baseline_devices_playbook.yaml

--d inventory

--f switch_devices.yaml

--d group_vars

--d devices_to_baseline

--f vars.yaml

--f vault.yaml

--d host_vars

--f bldg123_access_switch.yaml

--f bldg500_leaf_switch.yaml

--d templates

--f switch_baseline.j2

We will create all of these files and directories as part of this guide. In Linux, create the folders with the "mkdir" command. We also want to make all subfolders, and we can do that by using the "-p" or "--parents" flag and using the command with an expanded path e.g. "./inventory/group_vars" versus a single item path e.g. "./group_vars"

Make sure that your current working directory is still "ansible_for_networks". Type in the following commands to create the reference files and directories:

_**touch ./baseline_devices_playbook.yaml**_

_**mkdir -p ./inventory/group_vars/devices_to_baseline**_

_**touch ./inventory/switch_devices**__**.yaml**_

_**touch ./inventory/group_vars/**__**devices_to_baseline**__**/vars**__**.yaml**_

_**mkdir -p ./inventory/host_vars**_

_**touch ./inventory/host_vars/bldg123_access_switch.yaml**_

_**touch ./inventory/host_vars/bldg500_leaf_switch.yaml**_

_**mkdir ./templates**_

_**touch ./templates/switch_baseline.j2**_

Note, using the "touch" command only creates an empty file. We have to add our file contents over the next steps.

2.2.3 **Configuring Hosts.** In Ansible, the inventory is where information is stored about your network and systems environment. With plugins and prestaging, you could build thousands of devices into different host, group, and variable files for automation. For this exercise, we’ll build out a "./inventory/switch_devices.yaml" file with a list of devices we want to configure. Then, we’ll define a separate variable file for each device as "./host_vars/item_name.yaml" to track individual device settings. Finally, we’ll put shared variables in the "./group_vars/devices_to_baseline/vars.yaml" file.

Let’s start with the "./inventory/switch_devices.yaml" file. The filename could be whatever you want, we just have to reference its data from the playbook when we get to that point. In this case, we’re also setting it up as a "yaml" file which is just a data serialization storage format. Use the following configuration text as the reference for building this file. Use a text editor to add these lines into the file we created earlier. The root working directory is still "ansible_for_networks". Comments are included to explain relevant lines.

_**# A group of variables can be labeled with square brackets and a name**_

_**# Below, the all:vars tells Ansible that the variables will be used for all devices**_

_**[all:vars]**_

_**# This guide is only concerned with network devices, so set the connection for a network_cli**_

_**ansible_connection=network_cli**_

_**# Define a group of hosts under the name ‘devices_to_baseline’**_

_**# In the Ansible playbook, we will refer to this group name to**_ _**set a default templated baseline.**_

_**# The playbook will only operate on the hosts specified by its configuration**_

_**[devices_to_baseline]**_

_**# Hosts can be defined as an IP, a resolveable hostname, or an alias with an IP / hostname**_

_**#**_

_**# The following are valid entries:**_

_**#**_ _**192.168.15.2**_

_**#**_ _**my_device.lab_environment.localhost**_

_**#**_ _**my_local_pc ansible_host=127.0.0.1**_

_**#**_

_**# In this case, we are adding two hosts with aliases. The first string is the alias.**_

_**# e.g. with**_ _**"**__**ansible_host**__**"**__**, we assign an IP address to the alias**_ _**"**__**bldg123_access_switch**__**"**_

_**bldg123_access_switch**_ _**ansible_host=172.28.176.150**_

_**bldg500_leaf_switch ansible_host**__**=**__**172.28.176.1**__**60**_

Saving that file, next let’s build the "./inventory/group_vars" files. For this file, we’re defining variables that will be shared for all or most of the devices being managed by the playbook. This could be a group from a specific vendor, different hardware types, or devices that will fulfill a shared function like Layer2 or Leaf switches. In this case, we’re defining access and leaf switches that will have common VLANs. These variables specify that we'll be using Cisco IOS devices and we'll use "ansible_become" to elevate our privileges with the enable command. We’ll use the username "admin" and add a predefined set of VLANs onto each relevant device.

This file also introduces a line with an Ansible variable. The line with "ansible_ssh_pass" is followed by the value "{{ base_provision_password }}". This is how Ansible references a pre-defined variable that is pulled from its configuration or somewhere in the inventory. In this case, that variable will be defined in an Ansible vault. Ansible vaults allow us to store sensitive data in an encrypted file. This is a best practice as we should never store passwords or private API keys in plain text. After this file, we will be creating the Ansible vault for the devices_to_baseline group.

Open "./inventory/group_vars/devices_to_baseline/vars.yaml" and include the following:

_**---**_

_**# Variables for Provisioning Cisco IOS devices**_

_**ansible_network_os: ios**_

_**ansible_become: yes**_

_**ansible_become_method: enable**_

_**ansible_user: admin**_

_**ansible_ssh_pass:**_ _**"**__**{{ base_provision_password }}**__**"**_

_**vlans:**_

  _**- id: 10**_

    _**name:**_ _**"**__**VOICE**__**"**_

  _**- id: 50**_

    _**name:**_ _**"**__**DATA**__**"**_

  _**- id: 100**_

    _**name:**_ _**"**__**SERVICES**__**"**_

  _**- id: 124**_

    _**name:**_ _**"**__**PROVISIONING**__**"**_

  _**- id: 200**_

    _**name:**_ _**"**__**LAB**__**"**_

  _**- id: 600**_

    _**name:**_ _**"**__**IN_BAND_MANAGEMENT**__**"**_

  _**- id: 1000**_

    _**name:**_ _**"**__**UNUSED_SINKHOLE**__**"**_

  _**- id: 4094**_

    _**name:**_ _**"**__**LACP_MLAG**__**"**_

Next up, let’s create the vault for that group’s password. This uses the command "ansible-vault" to create a file with a password. For the purpose of this guide, we’ll use the password ‘admin’ to secure the vault and as the base provisioning password.

Open the file with the following command and add a password:

_**ansible-vault**_ _**create ./inventory/group_vars/**__**devices_to_baseline**__**/vault.yaml**_

Add the following text into the file, save the file, and then close the vault. Keep in mind that this variable name “base_provision_password” can be anything as required by the project. Just note that variables are case sensitive:

_**base_provision_password: admin**_

Moving on to the next files, we’re going to create our individual host variable files. These contain unique values that are specific to individual devices. These would be configuration items like management IPs, hostnames, Loopback addresses, or a unique BGP AS. Note, as we’re building our Ansible inventory, keep in mind that variables won’t be used automatically. Each of these files can serve as the source data for a template or playbook, but we have to reference the variables explicitly for that to happen.

That said, create the following files in the "./inventory/host_vars" directory. We’ll start with "./inventory/host_vars/bldg123_access_switch.yaml". Open this file for editing and add the following lines:

_**# host_vars/bldg123_access_switch.yaml (this is a yaml vars file for Ansible)**_

_**---**_

_**interfaces:**_

  _**- name: "Management1"**_

    _**enable: yes**_

    _**description: "Uplink to OOB_MGMT_SW ; to G1/0/1 ; OOB Device Management"**_

    _**layer3_port: yes**_

    _**ip_addr: "172.28.176.150 255.255.255.0"**_

  _**- name: "Ethernet1"**_

    _**enable: yes**_

    _**description: "User A Access Port ; to NIC_001 ; Data VLAN"**_

    _**vlan_mode: "untagged"**_

    _**vlan_id: "50"**_

    _**include_voice: yes**_

    _**voice_id: "10"**_

  _**- name: "Ethernet2"**_

    _**enable: yes**_

    _**description: "Webserver Access Port ; to NIC_003 ; Services VLAN"**_

    _**vlan_mode: "untagged"**_

    _**vlan_id: "50"**_

  _**- name: "Ethernet3"**_

    _**enable: yes**_

    _**description: "Uplink to bldg123_distro_switch ; to G1/0/50 ; 802.1Q Tagging VLANs"**_

    _**vlan_mode: "tagged"**_

    _**vlan_id: "10,50,100,124,200,600,4094"**_

  _**- name: "Vlan600"**_

    _**enable: yes**_

    _**layer3_port: yes**_

    _**description: "In-band VLAN for remote access and management"**_

    _**ip_addr: "10.10.10.1 255.255.255.240"**_

Now do the same for the second switch. Open the file "./inventory/host_vars/bldg500_leaf_switch.yaml" and add the following lines:

_**# host_vars/bldg500_leaf_switch.yaml (this is a yaml vars file for Ansible)**_

_**---**_

_**interfaces:**_

  _**- name:**_ _**"**__**GigabitEthernet1/0/1**__**"**_

    _**enable: yes**_

    _**description:**_ _**"**__**P2P Link to Spine-01 ; to G1/0/5 at 10.50.10.2 ; BGP Peer**__**"**_

    _**layer3_port: yes**_

    _**ip_addr:**_ _**"**__**10.50.10.1 255.255.255.252**__**"**_

    _**mtu: 9214**_

  _**- name:**_ _**"**__**GigabitEthernet1/0/2**__**"**_

    _**enable: yes**_

    _**description:**_ _**"**__**P2P Link to Spine-02 ; to G1/0/5 at 10.50.10.6 ; BGP Peer**__**"**_

    _**layer3_port: yes**_

    _**ip_addr:**_ _**"**__**10.50.10.5 255.255.255.252**__**"**_

    _**mtu: 9214**_

  _**- name:**_ _**"**__**GigabitEthernet1/0/3**__**"**_

    _**enable: yes**_

    _**description:**_ _**"**__**P2P Link to Spine-03 ; to G1/0/5 at 10.20.10.10 ; BGP Peer**__**"**_

    _**layer3_port: yes**_

    _**ip_addr:**_ _**"**__**10.50.10.9 255.255.255.252**__**"**_

    _**mtu: 9214**_

  _**- name:**_ _**"**__**GigabitEthernet2/0/1**__**"**_

    _**enable: yes**_

    _**description:**_ _**"**__**Webserver Access Port ; to NIC_001 ; Services VLAN**__**"**_

    _**vlan_mode:**_ _**"**__**untagged**__**"**_

    _**vlan_id:**_ _**"**__**50**__**"**_

  _**- name:**_ _**"**__**Loopback0**__**"**_

    _**enable: yes**_

    _**description:**_ _**"**__**Router ID and**_ _**Multicast Source / RP**__**"**_

    _**layer3_port: yes**_

    _**ip_addr:**_ _**"**__**192.168.30.5**_ _**255.255.255.25**__**5**__**"**_

_**enable_routing: yes**_

_**enable_bgp: yes**_

_**bgp_as:**_ _**"**__**65020**__**"**_

_**bgp_id:**_ _**"**__**192.168.30.5**__**"**_

After editing and saving both host_var files, we’ve staged example variables that are needed for individualized templating of our devices. Each of these files can be replicated and edited to include the details for an entire campus or data center of devices. This creates a baseline templating system that doesn’t require manual changes to whole configs. Instead, Ansible will insert the appropriate individual details by using a template that references those values.

## 2.3 Configuring a Jinja Configuration Template

2.3.1 **A Brief Overview of Jinja**. Jinja is a templating engine that was built for the Python language. Developed around 2008, it is widely used for everything from building web pages to generate device configuration files. The engine allows for variable processing, conditional statement evaluation, and loops to iterate through lists and dictionaries. Anyone familiar with Python should have an easy experience picking up Jinja and creating custom templates.

2.3.1.1 Writing a template is as easy as pasting in some known good text and using variable calls to inject custom values. For a network device, consider the following example: "hostname {{ device.hostname }}". Though all this does is set the device hostname, this is a valid Jinja template. When paired with Ansible, this template will be loaded and Ansible will use data from the inventory in an attempt to apply the referenced variable "device.hostname".

2.3.1.2 Logic capabilities make Jinja templates extremely flexible and powerful. We can adjust settings based on data from a target device, use pre-staged variables to adjust template output for multiple hosts, or send a repetitive command by looping through a list of different values. Using Python’s standard syntax, Jinja identifies logic evaluation by surrounding them with the brackets "{% evaluate %}". Without these brackets, Jinja will output exactly what’s written in the template. As an example, we could write an if / else statement like so:

{% if item.enable | default(false) %}

    no shutdown

{% else %}

    shutdown

{% endif %}

In this example, first we test if the variable "item.enable" is set to yes or no or true or false. Using the pipe, we’ve also explicitly defined that Jinja should consider this value as false if the variable doesn’t exist. If "item.enable" is true, Jinja will output the value "no shutdown" into the template. Otherwise, the value will be "shutdown" and the port will be deactivated. A default value will allow for dynamic values without having to explicitly define them every time. Defaults also prevent Ansible from failing due to errors from undefined variables. Whenever scripting out variables, it is best practice to plan for improperly set values. Always validate a variable’s value or, at a minimum, create a logic flow that can handle unexpected values.

2.3.1.3 Loops make it easy to create templates that include repetitive configuration items that only have a few small changes between each item. On network devices, loops are especially useful for things like interfaces, VLANs, or access lists / firewall filters. The interfaces defined in our host_vars files are a great example of using loops. In the Jinja template, we’ll use the "interfaces" object variable and pass the data into a loop like so:

{% for item in hostvars[inventory_hostname].interfaces %}

    interface {{ item.name }}

    {% if item.description is defined %}

        description {{ item.description }}

    {% endif %}

    {% if item.layer3_port | default(false) %}

        no switchport

    {% endif %}

    {% if item.ip_addr is defined %}

        ip address {{ item.ip_addr }}

    {% endif %}

{% endfor %}

On the top line, we tell Jinja to use a variable from the hostvars collected by Ansible from inventory values. The "inventory_hostname" is a variable referencing the specific hostname of the current device being evaluated by Ansible. Therefore, "hostvars[inventory_hostname].interfaces" passes the Python dictionary object from one of our two host_vars files into the for loop. Then, Jinja outputs configuration lines based on the values for each interface defined in that file. Whether there are two or twenty interfaces, Jinja will output lines for as many as are defined.

2.3.2 **Configuring a Jinja Baseline Template.** With those explanations out of the way, let’s define a Jinja template that can baseline network switches. Open the file "./templates/switch_baseline.j2" and enter the following lines:

_**! templates/switch_baseline.j2**_

_**! The**__**se**_ _**two lines instruct Jinja**_ _**not to trim or strip empty space**_

_**! This means it will output lines exactly as they are written in the template file**_

_**#jinja2: trim_blocks: False**_

_**#jinja2: lstrip_blocks: False**_

_**{% for vlan in vlans %}**_

_**vlan {{ vlan.id }}**_

  _**{% if vlan.name is defined %}**_

    _**name {{ vlan.name }}**_

  _**{% endif %}**_

_**{% endfor %}**_

_**{% for item in hostvars[inventory_hostname].interfaces %}**_

_**interface {{ item.name }}**_

  _**{% if item.description is defined %}**_

    _**description {{ item.description }}**_

  _**{% endif %}**_

  _**{% if**_ _**item.layer3_port | default(false)**_ _**%}**_

    _**no switchpor**__**t**_ 

    _**{% if item.ip_addr is defined %}**_

      _**ip address {{ item.ip_addr }}**_

    _**{% endif %}**_

    _**{% if it**__**em**__**.mtu is defined %}**_

      _**mtu {{ item.mtu }}**_

    _**{% endif %}**_

  _**{% else %}**_

    _**{% if item.**__**vlan_mode**_ _**is defined %}**_

      _**{% if item.vlan_mode ==**_ _**"**__**untagged**__**"**_ _**%}**_

        _**switchport mode access**_

        _**switchport access vlan {{ item.vlan_id }}**_

      _**{% endif %}**_

      _**{% if item.vlan_mode ==**_ _**"**__**tagged**__**"**_ _**%}**_

        _**switchport mode trunk**_

        _**switchport trunk allow vlan {{ item.vlan_id }}**_

      _**{% endif %}**_

      _**{% if item.include_voice | default(false) %}**_

        _**switchport voice vlan {{ item.vlan_id }}**_

      _**{% endif %}**_

    _**{% endif %}**_

  _**{% endif %}**_

  _**{% if item.enable | default(**__**false**__**) %}**_

    _**no shutdown**_

  _**{% else %}**_

    _**shutdown**_

  _**{% endif %}**_

_**{% endfor %}**_

_**{% if enable_routing | default(false) %}**_

  _**ip routing**_

  _**{% if enable_bgp | default(false) %}**_

    _**{% if bgp_as is defined %}**_

      _**router bgp {{ bgp_as }}**_

       _**{% if bgp_id is defined %}**_

         _**router-id {{ bgp_id }}**_

      _**{% endif %}**_

    _**{% endif %}**_

  _**{% endif %}**_

_**{% endif %}**_

As an overview, these lines instruct Jinja to create any VLANs defined for each device and any interfaces defined as well. For VLANs, the template outputs a configuration line to create and name each VLAN. For the interfaces, the template sets them as a Layer3, tagged, or untagged interface. Then it sets a description, IP address, and VLAN assignments as necessary and if defined. Finally, it shuts or no shuts the port based on the enable variable. Template defined, it’s time to create a playbook.

## 2.4 The Ansible Playbook

2.4.1 **Introduction to Playbooks**. Every Ansible playbook can be defined for a specific purpose or for broad and generalized configuration procedures. In this guide, we’re defining a playbook that will configure devices to a known-good baseline. Such a playbook could be used to provision new switches or to return fielded devices to an expected state.

With a playbook, we could just manually define each configuration line in the base file without using any variables or templates. However, that means having hundreds of repeated configuration lines for each device or group of devices. That would require manually changing each file for each device. Variables and templates allow us to make one or two changes in a way that will affect all devices. With a well-planned inventory, this method also makes it easy to provision dozens or hundreds of new devices on the fly and without changing more than a few lines per system.

2.4.2 **Defining a Playbook.** A playbook consists of three main items to work: what devices are we working with, what tasks will be performed for those devices, and what data will Ansible use to fulfill those tasks. In our inventory, we created the file "./inventory/switch_devices.yaml". This file tells Ansible to connect to these devices using the "network_cli" plugin and defines a group of devices under the "devices_to_baseline" group name. We can tell Ansible to operate on that entire group and it will iterate through each device hostname and IP. It will use files in the group_vars directory that match this group name, and it will use files in the host_vars directory to match the hostname, IP, or alias for each device in each group. Knowing that, let’s define the playbook by editing the file "./baseline_devices_playbook.yaml". Reference the following configuration lines:

_**# A human-readable name for descriptive purposes**_

_**- name: Baseline Cisco Network Devices**_

  _**# Tell ansible which hosts / devices to run this playbook for**_

  _**hosts: devices_to_baseline**_

  _**# For this guide, we are not going to query information about the environment or remote devices**_

  _**gather_facts: no**_

  _**# Run the following tasks for each device in the**_ _**"**__**devices_to_baseline**__**"**_ _**group**_

  _**tasks:**_

    _**# Tasks should have a descriptive name that summarizes the action to be performed**_

    _**- name: Configure hostname**_

      _**# We can run direct commands on a device with plugins like ios_config or eos_config**_

      _**# Commands must follow Network OS syntax and can use any variables pulled by Ansible**_

      _**ios_config:**_

        _**lines:**_

          _**# Change the device hostname**_

          _**-**_ _**"**__**hostname {{ inventory_hostname }}**__**"**_

    _**- name: Configure the device with a generalized baseline template**_

      _**# Ansible will attempt to automatically pull data from all applicable sources, but**_

      _**# we can also explicitly define the variables to use**_

      _**vars:**_

        _**group_data:**_ _**"**__**{{ groups[**__**'**__**devices_to_baseline**__**'**__**] }}**__**"**_

      _**ios_config:**_

        _**# Tell Ansible to run the Jinja template we defined for each device**_

        _**src: templates/switch_baseline.j2**_ 

2.4.3 **Staging Devices****.** At this point, we need to make sure our network devices are staged and reachable. To operate on a device, Ansible needs to be able to connect over SSH with pre-staged credentials. At a minimum, that means configuring an IP address, SSH credentials, and SSH settings. With the right setup, those settings could be staged using some form of zero touch provisioning. For this guide, we’ll paste in the changes manually.

2.4.3.1 This guide is built with the following lab setup. However, IP Addresses, usernames, passwords, and port designations should reflect the requirements of your environment. Make sure to change any templates or variables to match the setup being used relevant to your systems. Also ensure that the host where Ansible is installed is on the same network as the devices to be managed.

![](data:image/png;base64,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)

2.4.3.2 Use the following configuration as a reference for enabling remote access on both devices. Log into the devices via a console connection, **enable**, and enter global configuration mode with the **configuration terminal** command. Modify and then input the configuration lines as required.

_**username admin privilege 15 algorithm-type scrypt secret admin**_

_**!**_

_**aaa new-model**_

_**aaa authentication login default local**_

_**aaa authorization exec default local if-authenticated**_

_**aaa authorization console**_

_**!**_

_**!**_

_**ip domain-name lab.local**_

_**ip ssh version 2**_

_**crypto key generate rsa modulus 2048**_

_**!**_

_**interface GigabitEthernet0/0**_

_**description**_ _**Uplink to OOB_MGMT_SW ; to**_ _**G1/0**__**/**__**X ; OOB**_ _**Device**_ _**Management**_

_**ip address 172.28.176.1XX 255.255.255.0**_

_**no shutdown**_

_**!**_

_**ip access-list extended SSH**_

_**permit tcp 172.28.176.0 0.0.0.255 any eq 22 log**_

_**deny   ip any any log**_

_**!**_

_**line con 0**_

_**exec-timeout 5 0**_

_**logging synchronous**_

_**transport output none**_

_**line aux 0**_

_**transport output none**_

_**line vty 0 4**_

_**access-class SSH in**_

_**exec-timeout 5 0**_

_**logging synchronous**_

_**transport input ssh**_

_**transport output ssh**_

_**line vty 5 1500**_

_**access-class SSH in**_

_**logging synchronous**_

_**transport input none**_

_**transport output none**_

2.4.4 **Running the Playbook.** Once it’s defined, we can immediately run the playbook. The base command is "ansible-playbook -i inventory <playbook_file>". This tells Ansible to reach out to the devices defined in our inventory and change their configurations based on any templates and variables. However, we have a few additional requirements to consider.

2.4.4.1 First, let’s test the playbook by using the "dry run" feature of Ansible. Enable this feature by passing the "--check" command flag from the command line. Start our pipenv with the command **pipenv** **shell** and then run this command for our playbook like so:

_**ansible-playbook**_ _**-i inventory baseline_devices_playbook.yaml**_ _**--**__**check --ask-vault-pass**_

If all goes well, Ansible will output the changes it will make and whether it connected successfully to each device. It will also provide a "PLAY RECAP" that summarizes the number of devices connected, how many changes were made, and the number of errors.

![](data:image/png;base64,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)

2.4.4.2 During the dry run phase, double-check the output for each device. This is a good time to validate that the right things will be changed. In particular, operating on remote devices means a playbook could disconnect you from the system you’re managing. Take this time to correct any errors, troubleshoot template misconfigurations, or validate settings like IPs and port names. However, if all looks as it should, run the actual operation with:

_**ansible-playbook -i inventory baseline_devices_playbook.yaml --ask-vault-pass**_

2.4.4.3 Ansible will go through the inventory devices and implement the changes or each device. At this point, the general setup and implementation is complete. These same processes can be used to make additional playbooks, hosts, variables, and templates.

 

# 3 Wrapping Up

## 3.1 Limitations

As noted earlier, using Ansible does require connectivity to the devices in the first place. In a way, this might feel like it defeats the purpose of automation. After all, if you have to manually setup the device, you might as well paste in a good config at the time of provisioning. However, devices still need maintenance and upkeep after provisioning. New circuits need to be added, user ports will change, and security measure have to be adjusted. Plus, tools like Ansible can make replacing a device easier by allowing for small changes to variables rather than whole baseline configuration files.

Additionally, automation tools like Ansible can be tied into zero touch provisioning capabilities for a truly automated experience. Most modern network hardware has ZTP enabled at some level, so building in that automatic boot setup would be the first step to making playbooks even more valuable.

## 3.2 Next Steps

When building network automation capabilities, think through the lifecycle of an organization’s network hardware. As an example, how would we fully automate replacing the equipment for an entire campus and its supporting data center? First, the systems could be staged in racks and connected through an OOB network. Next, a ZTP process would provision these devices for remote connectivity. After that, Ansible could finalize each device with predefined templates aligned to purposes and locations.

Finally, the devices would be pulled into a network manager or SDN controller to enable overlay traffic. Once everything reports in and the network converges, swap over users and services to the new infrastructure. Spend time on toolsets that enable this kind of holistic approach to the organization’s network and it will pay dividends in saved time, money, and resources.

Ideally, we should also look for ways to operationalize the use of tools like Ansible. In essence, how do we prove the value of tweaking technical buttons and knobs? To do that, our efforts have to enable monitoring, reporting, and security. That may be through integrations with network vendor suites, Red Hat’s Ansible Tower, or we could develop custom packages built with a GUI or web stack. That’s the way to gather metrics on the benefits of our automation efforts.

A toolset that operates in a vacuum is doomed to be ignored and replaced. Leadership has to buy into technical developments to guarantee your focus and prioritization on those developments. It’s easy to get absorbed into fiddling and making the next cool thing, but make sure to take some time to showcase what that means to the organization.

## 3.3 Conclusion

This guide was written to provide a well-rounded intro to setting up Ansible, creating an inventory, and running playbooks with basic variables and vaults. The intent was to start from the ground floor and move all the way up to a working templated playbook. With these basics, even basic modifications can provide nearly-infinite combinations for managing networks and systems. Hopefully, it’s enough to get processes started to make everyday life just a little bit easier. Thanks for reading. 

References

Gonzalez, Noe (2022, April 26). How to Install Ansible with pipenv and pyenv. Retrieved on October 15, 2023 from https://www.buildahomelab.com/2022/04/26/how-to-install-ansible-with-pipenv-pyenv/

JetBrains (2023, September 27). Configure a pipenv environment. Retrieved on October 8, 2023 from https://www.jetbrains.com/help/pycharm/pipenv.html

Python Software Foundation (2022, April 28). Jinja2 3.1.2 - A very fast and expressive template engine. Retrieved on October 11, 2023 from https://pypi.org/project/Jinja2/

Red Hat (2021, September 15). What is Ansible. Retrieved on October 3, 2023 from https://www.redhat.com/en/technologies/management/ansible/what-is-ansible

Rometsch, Ben (2021, February 16). How Ansible got started and grew. Retrieved on October 11, 2023 from https://opensource.com/article/21/2/ansible-origin-story