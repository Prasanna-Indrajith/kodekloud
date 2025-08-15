# Troubleshoot and create playbook

## Question

An Ansible playbook needs completion on the jump host, where a team member left off. Below are the details:

The inventory file /home/thor/ansible/inventory requires adjustments. The playbook must run on App Server 3 in Stratos DC. Update the inventory accordingly.

Create a playbook /home/thor/ansible/playbook.yml. Include a task to create an empty file /tmp/file.txt on App Server 3.

Note: Validation will run the playbook using the command ansible-playbook -i inventory playbook.yml. Ensure the playbook works without any additional arguments.

## Answer

`cd` into Ansible folder and change the inventory file's details to match with app server 03. Create **playbook.yml** file.
```bash
cd Ansible

vi inventory
    - stapp03 ansible_host=172.16.238.12 ansible_user=banner ansible_ssh_common_args='-o StrictHostKeyChecking=no'
    - save and exit

vi playbook.yml
    ---
- name: Create empty file on App Server 3
  hosts: stapp03
  tasks:
    - name: Create /tmp/file.txt
      file:
        path: /tmp/file.txt
        state: touch
```

Secure ssh connection setup for passwordless login and automate.
```bash
ssh-keygen -t rsa

ssh-copy-id -i .ssh/id_rsa.pub banner@stapp03
```

Now run the ansible playbook
```bash
ansible-playbook -i inventory playbook.yml
```

## Brief Description About the Environment

**What is ansible and basic automation task?**

Ansible is an open-source automation tool used for configuration management, application deployment, and task automation across multiple systems. It uses simple YAML files called playbooks to define automation tasks and communicates over SSH, requiring no agent installation on target machines.

A basic automation task with Ansible could be installing a package on remote servers. For example, a playbook to install nginx:

```yaml
- hosts: webservers
  become: yes
  tasks:
    - name: Install nginx
      apt:
        name: nginx
        state: present
```

This playbook installs nginx on all hosts in the "webservers" group.
