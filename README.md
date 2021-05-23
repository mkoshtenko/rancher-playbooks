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
done:
- Install and run NTP daemon to keep the time in sync.
- Disable the swap file.

todo:
- configure ports
- configure hostname
- install modules
- install packets

## Usage
All the commands below should be executed on the control node only.
They assume that ansible_user can run sudo without a password.

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

#### Make sure the nodes are configured properly.

Hostnames are correct:
```
ansible rancher -a "hostname"
```

The nodes have enough free resources to run applications:
```
ansible rancher -a "df -h"
```

#### Run the playbook:
```
ansible-playbook playbook.yml
```

