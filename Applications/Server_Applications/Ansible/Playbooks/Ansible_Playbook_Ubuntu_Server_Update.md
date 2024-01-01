---
title: Ansible Playbook for updating ubuntu servers
description: Ansible Playnook to update ubuntu servers with ucaresystem-core. If the program is not installed it will install it
dateCreated: 
published: 
editor: markdown
tags: 
dateModified: 
---
# Ansible_Playbook_Ubuntu_Server_Update

server-software-update.yml

```
---
- name: Check if ucaresystem-core is installed
  hosts: ubuntu
  become: true
  become_user: root
  vars:
    ansible_become_pass: XXXXXXXX

  tasks:
    - name: Check if ucaresystem-core is installed
      command: dpkg -s ucaresystem-core
      register: ucaresystem_core_status
      ignore_errors: true

- name: Install ucaresystem-core if not present
  hosts: ubuntu
  become: true
  become_user: root
  vars:
    ansible_become_pass: XXXXXXXX

  tasks:
    - name: Add ucaresystem PPA
      apt_repository:
        repo: ppa:utappia/stable
        state: present

    - name: Update sources list
      apt:
        update_cache: yes

    - name: Install ucaresystem-core
      apt:
        name: ucaresystem-core
        state: present
      when: ucaresystem_core_status.rc != 0

- name: Run ucaresystem-core command
  hosts: ubuntu
  become: true
  become_user: root
  vars:
    ansible_become_pass: XXXXXXXX

  tasks:
    - name: Run ucaresystem-core
      command: ucaresystem-core
```
