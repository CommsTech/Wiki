---
title: NIX
description: NIX
dateCreated: 2022-09-09T04:44:01.149Z
published: true
editor: markdown
tags: NIX, Package Manager
dateModified: 
---
# Nix Package Manager

## Installation

Source: [https://github.com/NixOS/nix](https://github.com/NixOS/nix)

```fallback
curl -L https://nixos.org/nix/install | sh
```

Copy

_Note: I’d recommend multi-user install if it prompts for it._

## Finding Packages

I’d recommend using their website to find packages to install, but make sure to click the “unstable” button has NixOS stable is a Linux Distribution few use.

[https://search.nixos.org/packages](https://search.nixos.org/packages)

Or from terminal you can list all packages with `nix-env -qaP` then just [[grep]] what you are looking for. Example: `nix-env -qaP | grep hugo`

## Usage

Here is the basic usage of nix, most revolve around the `nix-env` command. These are manually managed and require user intervention

-   List Installed packages `nix-env -q`
-   Install Packages `nix-env -iA nixpkgs.packagename`
-   Erase Packages `nix-env -e packagename`
-   Update All Packages `nix-env -u`
-   Update Specific Packages `nix-env -u packagename`
-   Hold Specific Package `nix-env --set-flag keep true packagename`
-   List Backups (Generations) `nix-env --list-generations`
-   Rollback to Last Backup `nix-env --rollback`
-   Rollback to Specific Generation `nix-env --switch-generation #`

## Help and Manual

Official Manual is [here](https://nixos.org/manual/nix/stable/). You can also get more details with `man nix` or `man nix-env`.

## Troubleshooting

### Programs not showing up in start menu

NIX stores all the .desktop files for the programs it installs @ `/home/$USER/.nix-profile/share/applications/` and a simple symlink will fix them not showing up in your start menu.

```fallback
ln -s /home/$USER/.nix-profile/share/applications/* /home/$USER/.local/share/application
```