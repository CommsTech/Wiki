---
title: 
description: 
dateCreated: 
published: 
editor: markdown
tags: 
dateModified: 
---
# ansible_playbook_error_handling

Ansible is a very powerful tool that allows you to automate a huge variety of platforms across servers, clouds, networks, containers, and more.

Often you will be able to automate what you want by simply reusing existing [roles](https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse_roles.html) and [collections](https://docs.ansible.com/ansible/latest/user_guide/collections_using.html).

And there are many [modules](https://docs.ansible.com/ansible/2.9/modules/list_of_all_modules.html) to choose from that you can use in your playbooks.

But when you start developing and testing more complex playbooks, you will eventually need some troubleshooting methods. Things like:

- Checking the flow of the Ansible tasks.
- Confirming the data types of your variables.
- Even pausing at some particular point to inspect their values.

Some of the tips discussed in this article will only apply to Ansible execution via the command line. Others are also valid when running from Ansible [Tower](https://www.ansible.com/products/tower).

## 1. Your Ansible environment and parameters

If you need to investigate why something isn't behaving as expected in your playbooks, it's usually good to start by checking your Ansible environment.

Which versions and paths of the Ansible binaries and Python are you running?

If you added OS packages or Python modules that are required by your playbooks, is the Ansible interpreter "seeing" them?

You can obtain this basic information in many different ways. From the command line, run the `ansible --version` command.

```shell
❯ ansible --version

ansible 2.9.10

  config file = /etc/ansible/ansible.cfg

  configured module search path = ['/home/admin/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']

  ansible python module location = /usr/lib/python3.6/site-packages/ansible

  executable location = /usr/bin/ansible

  python version = 3.6.8 (default, Mar 18 2021, 08:58:41) [GCC 8.4.1 20200928 (Red Hat 8.4.1-1)]
```

You can get the same info by running other Ansible commands such as `ansible-playbook` or `ansible-config` with the `--version` option.

In Ansible Tower, this information is shown if the job template is executed with VERBOSITY 2 (More verbose) or higher.

Besides the versions and location of the Ansible and Python binaries, it's always good to double-check the paths used for modules, including whether the execution is using an `ansible.cfg` file that isn't the default (i.e., not `/etc/ansible/ansible.cfg`).

For investigating options coming from a custom `ansible.cfg` file, you can execute the following from the command line:

```shell
❯ ansible-config dump --only-changed

DEFAULT_BECOME(/home/admin/ansible/ansible.cfg) = True

DEFAULT_BECOME_USER(/home/admin/ansible/ansible.cfg) = root

DEFAULT_FORKS(/home/admin/ansible/ansible.cfg) = 10

DEFAULT_HOST_LIST(/home/admin/ansible/ansible.cfg) = ['/home/admin/ansible/inventory']

DEFAULT_ROLES_PATH(/home/admin/ansible/ansible.cfg) = ['/home/admin/ansible/roles']

HOST_KEY_CHECKING(/home/admin/ansible/ansible.cfg) = False
```

As the name implies, this will list the parameters that are _different_ from the default ones.

## 2. Running in verbose mode

Running the playbooks in debug mode can be the next step to get more details about what is happening in the tasks and variables.

From the command line, you can add `-v` (or `-vv`, `-vvv`, `-vvvv`, `-vvvvv`). The highest verbosity levels can sometimes be too much information, so it's better to increase gradually in multiple executions until you get what you need.

Level 4 can help when troubleshooting connectivity issues and level 5 is useful for WinRM problems.

From Tower, you can select the **VERBOSITY** level from the job template definition.

**Note**: Remember to disable the debug mode after you resolve the situation because the detailed information is only useful for troubleshooting.

Also, in debug mode, the values of certain variables (passwords, for example) will be displayed unless you use the `no_log` option in the task, so erase the outputs when you are done.

## 3. Using debug to show variables

If you have a good idea about _where_ the problems might be, you can use a more surgical approach: Display only the variables you need to see.

```yaml
(...)

  - name: Display the value of the counter
     debug:
      msg: "Counter={{ counter }} / Data type={{ counter | type_debug }}"

(...)

TASK [Display the value of the counter] ****************************************************************************

ok: [node1] => {

    "msg": "Counter=42 / Data type=AnsibleUnicode"

}
```

_This_ is why I couldn't increment the counter. The filter `type_debug` shows that the data type is **text** and not **int** as I supposed.

_**[ You might also like: [Quick start guide to Ansible for Linux sysadmins](https://www.redhat.com/sysadmin/ansible-quick-start) ]**_

## 4. Making sure that vars have the right content and data type

You can use the **assert** module to confirm that the variables have the expected content/type and cause the task to fail if something is wrong.

The following playbook illustrates this:

```yaml
---

- name: Assert examples
  hosts: node1
  gather_facts: no
  vars:
    var01: 13
  tasks:

  - debug:
      msg: "Parameter 01 is {{ (param01 | type_debug) }}"

  - name: Make sure that we have the right type of content before proceeding
    assert:
      that: 

      - "var01 is defined"
      - "(var01 | type_debug) == 'int' "
      - "param01 is defined "
      - "(param01 | type_debug) == 'int' "
```

If I run the playbook from the command line, _without_ providing the extra-vars:

```shell
❯ ansible-playbook xassert.yml

PLAY [Assert examples] ****************************************************************************

TASK [debug] ****************************************************************************

ok: [node1] => {

    "msg": "Parameter 01 is AnsibleUndefined"

}

TASK [Make sure that we have the right type of content before proceeding] ****************************************************************************

fatal: [node1]: FAILED! => {

    "assertion": "param01 is defined ",

    "changed": false,

    "evaluated_to": false,

    "msg": "Assertion failed"

}

PLAY RECAP ****************************************************************************

node1 : ok=1 changed=0 unreachable=0 failed=1 skipped=0 rescued=0 ignored=0  
```

If I run the playbook from the command line, _with_ the extra-var:

```shell
❯ ansible-playbook xassert.yml -e "param01=99"

PLAY [Assert examples] ****************************************************************************

TASK [debug] ****************************************************************************

ok: [node1] => {

    "msg": "Parameter 01 is str"

}

TASK [Make sure that we have the right type of content before proceeding] ****************************************************************************

fatal: [node1]: FAILED! => {

    "assertion": "(param01 | type_debug) == 'int' ",

    "changed": false,

    "evaluated_to": false,

    "msg": "Assertion failed"

}

PLAY RECAP ****************************************************************************

node1 : ok=1 changed=0 unreachable=0 failed=1 skipped=0 rescued=0 ignored=0 
```

Seeing the data type coming as **str** was a surprise to me, but there is [an explanation and a solution](https://github.com/ansible/ansible/issues/14179) here.

Also, if you run the same playbook from Tower, either passing the parameter as an extra-vars or a field from a survey, the parameter's data type will be **int**.

Yes, it can be tricky... but if you know what to look for, these "features" won't be a problem for you.

## 5. Getting a list of the facts and vars

Whether you have defined variables in your inventory (for hosts and/or groups) or created and assigned additional variables during the execution of your playbook, it may be useful at some point to inspect their values.

```yaml
---
- name: vars
  hosts: node1,node2
  tasks:
 
  - name: Dump vars
    copy:
      content: "{{ hostvars[inventory_hostname] | to_nice_yaml }}"
      dest: "/tmp/{{ inventory_hostname }}_vars.txt"
    delegate_to: control

  - name: Dump facts
    copy: 
      content: "{{ ansible_facts | to_nice_yaml }}"
      dest: "/tmp/{{ inventory_hostname }}_facts.txt"
    delegate_to: control
```

This will save the vars and facts (if you are gathering facts) in individual files for you to analyze.

For the command line execution, I delegated the task to my control host (localhost) to have the files saved locally instead of having the files saved in each node separately. If running from Tower, you will also need to select _one_ server where to store the files (unless you have only one target node and don't mind analyzing the file there).

## 6. Using the task debugger for troubleshooting from the command line

You can also use the Ansible [debugger](https://docs.ansible.com/ansible/latest/user_guide/playbooks_debugger.html) to execute playbooks in a step-by-step mode and to examine the content of variables and arguments interactively.

Besides that, you can also change the values of variables on-the-fly, and continue the execution.

The debugger can be enabled at the task level or the play level, like in the following example:

```yaml
---

- name: Example using debugger
  hosts: localhost
  gather_facts: no
  vars:
    radius: "5.3"
    pi: "3.1415926535"
  debugger: on_failed
  tasks:

  - name: Calculate the area of a circle
    debug:
      msg:
      - "Radius.............: {{ radius }}"
      - "pi................: {{ pi }}"
      - "Area of the circle: {{ (pi * (radius * radius)) }}"
```

Spoiler alert: I defined the variables as strings, which will obviously cause an error when I try to do the calculation.

```shell
❯ ansible-playbook xdebugger.yml 

PLAY [Example using debugger] ****************************************************************************

TASK [Calculate the area of a circle] ****************************************************************************

fatal: [localhost]: FAILED! => {"msg": "Unexpected templating type error occurred on (Area of the circle: {{ (pi * (radius * radius)) }}): can't multiply sequence by non-int of type 'AnsibleUnicode'"}

[localhost] TASK: Calculate the area of a circle (debug)> p task_vars['pi']

'3.1415926535'

[localhost] TASK: Calculate the area of a circle (debug)> p task_vars['radius']

'5.3'

[localhost] TASK: Calculate the area of a circle (debug)> task_vars['pi']=3.1415926535

[localhost] TASK: Calculate the area of a circle (debug)> task_vars['radius']=5.3

[localhost] TASK: Calculate the area of a circle (debug)> p task_vars['radius']

5.3

[localhost] TASK: Calculate the area of a circle (debug)> task_vars['pi']=3.1415926535

[localhost] TASK: Calculate the area of a circle (debug)> redo

ok: [localhost] => {

    "msg": [

        "Radius............: 5.3",

        "pi................: 3.1415926535",

        "Area of the circle: 88.247337636815"

    ]

}

PLAY RECAP ****************************************************************************

localhost : ok=1 changed=0 unreachable=0 failed=0 skipped=0 rescued=0 ignored=0  
```

What happened here:

1. Initially, the task failed, complaining about the non-int variables.
2. The debugger was invoked.
3. I used the print (**p**) command to display the values of the variables.
4. In this case, I knew that the problem was in the data type, but one could think the values are correct (if not paying attention to the quotes around the values).
5. Later, I updated the content of the variables, assigning numbers to them.
6. Then, I used the `redo` command to re-execute the task with the new values, and it finished successfully.

This was a simple scenario, as we know that _no one_ would really use Ansible to calculate the area of a circle. But in a more complex situation, it could be useful to find the content of a variable in the middle of a long playbook execution, and be able to proceed from that point without restarting from scratch. 

## Wrap up

Incorporating a good arsenal of troubleshooting options can help you identify issues in your Ansible playbooks more quickly. Depending on where you are in the investigation, certain methods are more suitable.

For example, when you're just trying to get a sense of _what_ could be wrong, and _where_, you may want to start with gradually increasing the debug level.

Once you have a better idea of the location of the problem, it may be convenient to decrease the debug level (so you have less output in front of you) and use options that are specific to the task you are analyzing.

Troubleshooting Ansible can be tricky but using a methodical approach combined with the built-in problem-solving tools you can make it far easier on yourself. Confirm the Ansible environment and task flow, then look for proper data types, and finally consider pausing and stepping through each task.