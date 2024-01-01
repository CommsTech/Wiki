---
title: Nvidia Cuda 11.4.2 Ubuntu Install
description: 
dateCreated: 
published: 
editor: markdown
tags: 
dateModified: 
---
# [Installing CUDA Toolkit v11.4.2 on Ubuntu](https://curiousstuff.eu/post/installing-cuda-toolkit-v11.4.2-on-ubuntu/)

Steps on how to install CUDA Toolkit v11.4.2 on Linux

[Sergio Anguita Lorenzo](https://es.linkedin.com/in/sergio-anguita)

Oct 07, 2021

Reading time: 4 minutes.

Table of Content

- [Installing CUDA Toolkit on Ubuntu](https://curiousstuff.eu/post/installing-cuda-toolkit-v11.4.2-on-ubuntu/#installing-cuda-toolkit-on-ubuntu)
- [Pre-installation Actions](https://curiousstuff.eu/post/installing-cuda-toolkit-v11.4.2-on-ubuntu/#pre-installation-actions)
    - [Verify the system has a CUDA-capable GPU](https://curiousstuff.eu/post/installing-cuda-toolkit-v11.4.2-on-ubuntu/#verify-the-system-has-a-cuda-capable-gpu)
    - [Verify the system is running a supported version of Linux](https://curiousstuff.eu/post/installing-cuda-toolkit-v11.4.2-on-ubuntu/#verify-the-system-is-running-a-supported-version-of-linux)
    - [Verify the system has `gcc` installed](https://curiousstuff.eu/post/installing-cuda-toolkit-v11.4.2-on-ubuntu/#verify-the-system-has-gcc-installed)
    - [Verify the system has the correct kernel headers and development packages installed](https://curiousstuff.eu/post/installing-cuda-toolkit-v11.4.2-on-ubuntu/#verify-the-system-has-the-correct-kernel-headers-and-development-packages-installed)
    - [Download the NVIDIA CUDA Toolkit](https://curiousstuff.eu/post/installing-cuda-toolkit-v11.4.2-on-ubuntu/#download-the-nvidia-cuda-toolkit)
    - [Handle conflicting installation methods.](https://curiousstuff.eu/post/installing-cuda-toolkit-v11.4.2-on-ubuntu/#handle-conflicting-installation-methods)
    - [Using Debian installer](https://curiousstuff.eu/post/installing-cuda-toolkit-v11.4.2-on-ubuntu/#using-debian-installer)
    - [Using runfile installer](https://curiousstuff.eu/post/installing-cuda-toolkit-v11.4.2-on-ubuntu/#using-runfile-installer)
- [CUDA Toolkit Post-installation steps](https://curiousstuff.eu/post/installing-cuda-toolkit-v11.4.2-on-ubuntu/#cuda-toolkit-post-installation-steps)
    - [Install Persistence Daemon](https://curiousstuff.eu/post/installing-cuda-toolkit-v11.4.2-on-ubuntu/#install-persistence-daemon)
- [References](https://curiousstuff.eu/post/installing-cuda-toolkit-v11.4.2-on-ubuntu/#references)

This guide covers the basic instructions needed to install CUDA and verify that a CUDA application can run on Linux.

CUDA on Linux can be installed using an RPM, Debian, Runfile, or Conda package, depending on the platform being installed on.

## Installing CUDA Toolkit on Ubuntu

When installing CUDA on Ubuntu, you can choose between the Runfile Installer and the Debian Installer. The Runfile Installer is only available as a Local Installer. The Debian Installer is available as both a Local Installer and a Network Installer. The Network Installer allows you to download only the files you need. The Local Installer is a stand-alone installer with a large initial download. In the case of the Debian installers, the instructions for the Local and Network variants are the same. For more details, refer to the Linux Installation Guide.

## Pre-installation Actions

Some actions must be taken before the CUDA Toolkit and Driver can be installed on Linux:

- Verify the system has a CUDA-capable GPU.
- Verify the system is running a supported version of Linux.
- Verify the system has `gcc` installed.
- Verify the system has the correct kernel headers and development packages installed.
- Download the NVIDIA CUDA Toolkit.
- Handle conflicting installation methods.

### Verify the system has a CUDA-capable GPU

To verify that your GPU is CUDA-capable, go to your distribution’s equivalent of System Properties, or, from the command line, run:

|   |   |
|---|---|
|```<br>1<br>```|```bash<br>lspci \| grep -i nvidia<br>```|

Copy

It must return your detected Graphic Card

|   |   |
|---|---|
|```<br>1<br>```|```bash<br>01:00.0 VGA compatible controller: NVIDIA Corporation GP107GL [Quadro P620] (rev a1)<br>```|

Copy

### Verify the system is running a supported version of Linux

The CUDA Development Tools are only supported on some specific distributions of Linux. These are listed in the CUDA Toolkit release notes.

To determine which distribution and release number you’re running, type the following at the command line:

|   |   |
|---|---|
|```<br>1<br>```|```bash<br>uname -m && cat /etc/*release<br>```|

Copy

You should see output similar to the following, modified for your particular system:

|   |   |
|---|---|
|```<br> 1<br> 2<br> 3<br> 4<br> 5<br> 6<br> 7<br> 8<br> 9<br>10<br>11<br>12<br>13<br>14<br>15<br>16<br>17<br>```|```fallback<br>x86_64<br>DISTRIB_ID=Ubuntu<br>DISTRIB_RELEASE=18.04<br>DISTRIB_CODENAME=bionic<br>DISTRIB_DESCRIPTION="Ubuntu 18.04.6 LTS"<br>NAME="Ubuntu"<br>VERSION="18.04.6 LTS (Bionic Beaver)"<br>ID=ubuntu<br>ID_LIKE=debian<br>PRETTY_NAME="Ubuntu 18.04.6 LTS"<br>VERSION_ID="18.04"<br>HOME_URL="https://www.ubuntu.com/"<br>SUPPORT_URL="https://help.ubuntu.com/"<br>BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"<br>PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"<br>VERSION_CODENAME=bionic<br>UBUNTU_CODENAME=bionic<br>```|

Copy

### Verify the system has `gcc` installed

The `gcc` compiler is required for development using the CUDA Toolkit. It is not required for running CUDA applications. It is generally installed as part of the Linux installation, and in most cases the version of `gcc` installed with a supported version of Linux will work correctly.

To verify the version of `gcc` installed on your system, type the following on the command line:

|   |   |
|---|---|
|```<br>1<br>```|```bash<br>gcc --version<br>```|

Copy

|   |   |
|---|---|
|```<br>1<br>2<br>3<br>4<br>```|```fallback<br>gcc (Ubuntu 7.5.0-3ubuntu1~18.04) 7.5.0<br>Copyright (C) 2017 Free Software Foundation, Inc.<br>This is free software; see the source for copying conditions.  There is NO<br>warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.<br>```|

Copy

### Verify the system has the correct kernel headers and development packages installed

The kernel headers and development packages for the currently running kernel can be installed with:

|   |   |
|---|---|
|```<br>1<br>```|```bash<br>sudo apt-get install linux-headers-$(uname -r)<br>```|

Copy

### Download the NVIDIA CUDA Toolkit

The NVIDIA CUDA Toolkit is available at [https://developer.nvidia.com/cuda-downloads](https://developer.nvidia.com/cuda-downloads).

### Handle conflicting installation methods

Before installing CUDA, any previously installations that could conflict should be uninstalled. This will not affect systems which have not had CUDA installed previously, or systems where the installation method has been preserved

### Using Debian installer

Basically, the steps to install using this method are. Install the repository meta-data, install GPG key, update the apt-get cache, and install CUDA.

> Note that is `sudo` is installed on your system, you need to execute following commands with `sudo` and the current user must be in `sudoers`.

|   |   |
|---|---|
|```<br>1<br>2<br>3<br>4<br>5<br>6<br>7<br>```|```bash<br>wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/cuda-ubuntu1804.pin<br>sudo mv cuda-ubuntu1804.pin /etc/apt/preferences.d/cuda-repository-pin-600<br>wget https://developer.download.nvidia.com/compute/cuda/11.4.2/local_installers/cuda-repo-ubuntu1804-11-4-local_11.4.2-470.57.02-1_amd64.deb<br>sudo apt-key add /var/cuda-repo-ubuntu1804-11-4-local/7fa2af80.pub<br>sudo dpkg -i cuda-repo-ubuntu1804-11-4-local_11.4.2-470.57.02-1_amd64.deb<br>sudo apt-get update<br>sudo apt-get -y install cuda<br>```|

Copy

Now you to reboot the machine and setup your environment.

|   |   |
|---|---|
|```<br>1<br>2<br>```|```fallback<br>export PATH=/usr/local/cuda-11.4/bin${PATH:+:${PATH}}<br>export LD_LIBRARY_PATH=/usr/local/cuda-11.4/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}<br>```|

Copy

Finally, if you want the samples you can get them with

|   |   |
|---|---|
|```<br>1<br>2<br>3<br>4<br>```|```bash<br>cuda-install-samples-11.4.sh ~<br>cd ~/NVIDIA_CUDA-11.4_Samples/5_Simulations/nbody<br>make<br>./nbody<br>```|

Copy

> Remember that you have to run the samples by navigating to the executable’s location, otherwise it will fail to locate dependent resources.

### Using runfile installer

#### DISABLE Nouveau drivers

Create a file at `/etc/modprobe.d/blacklist-nouveau.conf` with the following contents

|   |   |
|---|---|
|```<br>1<br>2<br>```|```fallback<br>blacklist nouveau<br>options nouveau modeset=0<br>```|

Copy

Regenerate the kernel initramfs

|```bash 

sudo update-initramfs -u```|

Copy

After this step, reboot into runlevel 3 by temporarily adding the number `3` and the word `nomodeset` to the end of the system’s kernel boot parameters. Finally, run the installer:

|   |   |
|---|---|
|```<br>1<br>2<br>```|```bash<br>wget https://developer.download.nvidia.com/compute/cuda/11.4.2/local_installers/cuda_11.4.2_470.57.02_linux.run<br>sudo sh cuda_11.4.2_470.57.02_linux.run --silent<br>```|

Copy

Create an `xorg.conf` file to use the NVIDIA GPU for display:

|   |   |
|---|---|
|```<br>1<br>```|```bash<br>sudo nvidia-xconfig<br>```|

Copy

## CUDA Toolkit Post-installation steps

The post-installation actions must be manually performed. These actions are split into mandatory, recommended, and optional sections.

- Environment Setup
- Install Persistence Daemon

### Install Persistence Daemon

NVIDIA is providing a user-space daemon on Linux to support persistence of driver state across CUDA job runs. The daemon approach provides a more elegant and robust solution to this problem than persistence mode. For more details on the NVIDIA Persistence Daemon.

The NVIDIA Persistence Daemon can be started as the root user by running

|   |   |
|---|---|
|```<br>1<br>```|```bash<br>/usr/bin/nvidia-persistenced --verbose<br>```|

Copy

## References

- [https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html)
- [https://docs.nvidia.com/cuda/pdf/CUDA_Installation_Guide_Linux.pdf](https://docs.nvidia.com/cuda/pdf/CUDA_Installation_Guide_Linux.pdf)

---

### Subscribe, donate or become premium

You like it? Help making this blog better and subscribe to [get advantages](https://curiousstuff.eu/premium) or make a one-time donation.

[

**Buy me a coffee**

](https://www.buymeacoffee.com/zerjioang)