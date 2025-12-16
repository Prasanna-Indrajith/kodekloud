# Ansible Manage Services

## Question

Developers are looking for dependencies to be installed and run on Nautilus app servers in Stratos DC. They have shared some requirements with the DevOps team. Because we are now managing packages installation and services management using Ansible, some playbooks need to be created and tested. As per details mentioned below please complete the task:

a. On jump host create an Ansible `playbook` `/home/thor/ansible/playbook.yml` and configure it to **install** `httpd` on **all app servers**.

b. After installation make sure to **start** and **enable** `httpd` service on all app servers.

c. The inventory `/home/thor/ansible/inventory` is already there on jump host.

d. Make sure user thor should be able to run the playbook on jump host.

## Answer

1. **Create the playbook.yml**

```yml
---
- name: apache setup
  hosts: all
  become: yes

  tasks:
    - name: install apache
      ansible.builtin.yum:
        name: httpd
        state: present

    - name: Start apache
      ansible.builtin.service:
        name: httpd
        state: started
        enabled: yes
```

- inventory file looks like this

```ini
stapp01 ansible_host=172.16.238.10 ansible_ssh_pass=Ir0nM@n ansible_user=tony
stapp02 ansible_host=172.16.238.11 ansible_ssh_pass=Am3ric@ ansible_user=steve
stapp03 ansible_host=172.16.238.12 ansible_ssh_pass=BigGr33n ansible_user=banner
```

2. **Run the playbook**

   - `ansible-playbook -i inventory playbook.yml`
