---
title: Kubernetes Docker
description: 
tags:
  - Kubernetes
  - Virtualization
aliases:
---
# Installing docker on ubuntu server
## [Prerequisites](https://docs.docker.com/engine/install/ubuntu/#prerequisites) 

> **Note**
> 
> If you use ufw or firewalld to manage firewall settings, be aware that when you expose container ports using Docker, these ports bypass your firewall rules. For more information, refer to [Docker and ufw](https://docs.docker.com/network/packet-filtering-firewalls/#docker-and-ufw).

### [OS requirements](https://docs.docker.com/engine/install/ubuntu/#os-requirements) 

To install Docker Engine, you need the 64-bit version of one of these Ubuntu versions:

- Ubuntu Lunar 23.04
- Ubuntu Kinetic 22.10
- Ubuntu Jammy 22.04 (LTS)
- Ubuntu Focal 20.04 (LTS)

Docker Engine for Ubuntu is compatible with x86_64 (or amd64), armhf, arm64, s390x, and ppc64le (ppc64el) architectures.

### [Uninstall old versions](https://docs.docker.com/engine/install/ubuntu/#uninstall-old-versions) 

Before you can install Docker Engine, you must first make sure that any conflicting packages are uninstalled.

Distro maintainers provide an unofficial distributions of Docker packages in APT. You must uninstall these packages before you can install the official version of Docker Engine.

The unofficial packages to uninstall are:

- `docker.io`
- `docker-compose`
- `docker-doc`
- `podman-docker`

Moreover, Docker Engine depends on `containerd` and `runc`. Docker Engine bundles these dependencies as one bundle: `containerd.io`. If you have installed the `containerd` or `runc` previously, uninstall them to avoid conflicts with the versions bundled with Docker Engine.

Run the following command to uninstall all conflicting packages:

```console
$ for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done
```

`apt-get` might report that you have none of these packages installed.

Images, containers, volumes, and networks stored in `/var/lib/docker/` aren't automatically removed when you uninstall Docker. If you want to start with a clean installation, and prefer to clean up any existing data, read the [uninstall Docker Engine](https://docs.docker.com/engine/install/ubuntu/#uninstall-docker-engine) section.

## [Installation methods](https://docs.docker.com/engine/install/ubuntu/#installation-methods) 

You can install Docker Engine in different ways, depending on your needs:

- Docker Engine comes bundled with [Docker Desktop for Linux](https://docs.docker.com/desktop/install/linux-install/). This is the easiest and quickest way to get started.
    
- Set up and install Docker Engine from [Docker's Apt repository](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository).
    
- [Install it manually](https://docs.docker.com/engine/install/ubuntu/#install-from-a-package) and manage upgrades manually.
    
- Use a [convenience script](https://docs.docker.com/engine/install/ubuntu/#install-using-the-convenience-script). Only recommended for testing and development environments.
    

### [Install using the Apt repository](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository) 

Before you install Docker Engine for the first time on a new host machine, you need to set up the Docker repository. Afterward, you can install and update Docker from the repository.

1. Set up Docker's Apt repository.
    
    ```bash
    # Add Docker's official GPG key:
    sudo apt-get update
    sudo apt-get install ca-certificates curl gnupg
    sudo install -m 0755 -d /etc/apt/keyrings
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
    sudo chmod a+r /etc/apt/keyrings/docker.gpg
    
    # Add the repository to Apt sources:
    echo \
      "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
      "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
      sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    sudo apt-get update
    ```
    
    > **Note**
    > 
    > If you use an Ubuntu derivative distro, such as Linux Mint, you may need to use `UBUNTU_CODENAME` instead of `VERSION_CODENAME`.
    
2. Install the Docker packages.
    
    Latest Specific version
    
    ---
    
    To install the latest version, run:
    
    ```console
    $ sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
    ```
    
    ---
    
3. Verify that the Docker Engine installation is successful by running the `hello-world` image.
    
    ```console
    $ sudo docker run hello-world
    ```
    
    This command downloads a test image and runs it in a container. When the container runs, it prints a confirmation message and exits.
    

You have now successfully installed and started Docker Engine.