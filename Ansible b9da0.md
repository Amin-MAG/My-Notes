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

```
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

## Playbook

You can define some play book like

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

# References

[you need to learn Ansible RIGHT NOW!! (Linux Automation)](https://www.youtube.com/watch?v=5hycyr-8EKs)