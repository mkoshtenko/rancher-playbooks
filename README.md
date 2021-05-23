# rancher-playbooks
Ansible playbooks for Rancher node management

The repo contains playbooks to help with initial provisioning of rancher nodes. 

## Ansible
Ansible is an open-source software provisioning, configuration management, and application-deployment tool enabling infrastructure as code.
[Getting Started](https://docs.ansible.com/ansible/latest/user_guide/intro_getting_started.html)

Install and update on OS X (Control node)
```
brew install ansible
```
## Rancher
Rancher is open source software that combines everything an organization needs to adopt and run containers in production. Built on Kubernetes, Rancher makes it easy for DevOps teams to test, deploy and manage their applications.
[Getting Started](https://rancher.com/docs/rancher/v2.5/en/)

## Objectives
- disable swap
- configure ports
- configure hostname
- install modules
- install packets

## Usage
All the commands below should be executed on the control node only.

Add node to ansible hosts file (default: /etc/ansible/hosts)
```
# Rancher nodes
[etcd]
192.168.1.116

## Group for rancher nodes
[rancher:children]
etcd

## Variables that will be applied to all rancher nodes
[rancher:vars]
ansible_user=root
```

Let's make sure the nodes are configured properly.
Hostnames are correct:
```
ansible rancher -a "hostname"
```
The nodes have enough free resources to run applications:
```
ansible rancher -a "df -h"
```

The nodes have the right time and date:
```
ansible rancher -a "date"
```
Install NTP daemon on the nodes to keep the time in sync.
Note: this assumes ansible_user can run sudo without a password.
TODO: move to the playbook
```
ansible rancher -b -m apt -a "name=chrony state=present"
ansible rancher -b -m service -a "name=chronyd state=started enabled=yes"
ansible rancher -b -a "chronyd -q"
```



