# Ansible

## Overview

When you have a set of servers and want to apply some changes to it, you can just modify the Control Station.

Ansible is

- for Red Hat
- Python-Based
- Agentless
- Using SSH
- Free
- Push Configuration

## Configuration

For the controller node, You can see the Ansible configuration on the `/etc/ansible` directory. The `hosts` contains the machines, routers, or switches that you are going to control.

To add a group of hosts

```plain
[linux]
45.56.72.153
45.56.72.154

[linux:vars]
ansible_user=ans_user
ansible_password=pas123123
```

Now let’s do something with this group

```bash
# Use ping module for linux groups
ansible linux -m ping

# Execute a command
ansible linux -a "cat /etc/os-release"
```

## Concepts

### Control Node

Any machine with Ansible installed. You can have multiple control nodes.

### Managed Nodes

Hosts or managed nodes are the network devices that you manage using Ansible.

### Inventory

A list of managed nodes. An inventory file or `hostfile` is a file that stores information about the managed machines.

### Collection

Collections are a distribution format for Ansible content that can include playbooks, roles, modules, and plugins. You can install and use collections through [Ansible Galaxy](https://galaxy.ansible.com/).

### Module

The units of code Ansible executes. Each module has a specific usage.

### Task

The units of actions in Ansible. You can execute a single task once with an ad hoc command.

### Playbook

The playbook is ordered lists of tasks. Here is my initial playbook.

```yaml
- name: My Initial Playbook
  hosts: all
  tasks:
    - name: Create an empty file
      command: "touch ~/ansible_was_here.tmp"
```

To run a playbook, you should run this command.
```bash
ansible-playbook my_initial_playbook.yml
```

Here is another example.

```yaml
- name: Build Unicorn
  hosts: os-playground2

  vars:
    workdir: ~/Mesbah/unicorn
  environment:
    UNICORN_VERSION: "{{ lookup('env', 'CI_COMMIT_REF_NAME') }}"
    OSM_PBF_DATE: "{{ lookup('env', 'OSM_PBF_DATE') }}"

  tasks:
    - name: Git pull the latest codes
      command: git pull
      args:
        chdir: '{{ workdir }}'

    - name: Build the docker image
      command: "docker-compose build unicorn"
      args:
        chdir: '{{ workdir }}'
```

You can also group your hosts in `hosts.ini`

```yaml
[openstack]
os-playground1
os-playground2

[openstack:vars]
ansible_connection=ssh
ansible_shell_executable=/bin/bash
ansible_python_interpreter=/home/ubuntu/anaconda3/bin/python
```

You need and `ssh_config` as your credentials too

```yaml
Host os-bastion
	User ubuntu
	Hostname 172.21.50.35
	StrictHostKeyChecking no
	UserKnownHostsFile=/dev/null

Host os-playground2
	User ubuntu
	Hostname  172.16.69.38
	ProxyCommand ssh ubuntu@os-bastion -W %h:%p
	StrictHostKeyChecking no
	UserKnownHostsFile=/dev/null

Host os-playground1
	User ubuntu
	Hostname  172.16.69.32
	ProxyCommand ssh ubuntu@os-bastion -W %h:%p
	StrictHostKeyChecking no
	UserKnownHostsFile=/dev/null
```

By running this command you can executes the play books

```bash
# For ping pong test
ansible -i hosts.ini ${ANSIBLE_REMOTE_HOST} -m ping
ansible-playbook -i hosts.ini build-playbook.yml
```

## Commands

To configure your hosts and managed machines, you need to change `/etc/ansible/hosts`.

```plain
194.5.193.113 ansible_user=ubuntu
```

### Basic commands

To ping all your managed machines and test the connection. You should add your ssh key.

```bash
eval `ssh-agent`
ssh-add .ssh/mykey
ansible all -m ping
# 194.5.193.113 | SUCCESS => {
#     "ansible_facts": {
#         "discovered_interpreter_python": "/usr/bin/python3"
#     },
#     "changed": false,
#     "ping": "pong"
# }
```

Run a command on every node:

```bash
ansible all -a "/bin/echo hello-world!"
# 194.5.193.113 | CHANGED | rc=0 >>
# hello-world
```

### Ad hoc

Ad hoc commands are great for executing a command that you repeat rarely.

```bash
ansible [pattern] -m [module] -a "[module options]"

# To reboot all the servers in the [atlanta] group:
ansible atlanta -a "/sbin/reboot"

# To reboot the [atlanta] servers with 10 parallel forks:
ansible atlanta -a "/sbin/reboot" -f 10

# To connect as a different user
ansible atlanta -a "/sbin/reboot" -f 10 -u amin

# You can connect to the server as `username` and 
# run the command as the `root` user by using the become keyword.
ansible atlanta -a "/sbin/reboot" -f 10 -u amin --become-user batman -K

# All above examples are with command module, to use another 
# module:
ansible atlanta -m ansible.builtin.copy -a "src=/etc/hosts dest=/tmp/hosts"
ansible webservers -m ansible.builtin.file -a "dest=/srv/foo/a.txt mode=600"
```

# References

- [you need to learn Ansible RIGHT NOW!! (Linux Automation)](https://www.youtube.com/watch?v=5hycyr-8EKs)
- [Ansible concepts](https://docs.ansible.com/ansible/latest/user_guide/basic_concepts.html#tasks)