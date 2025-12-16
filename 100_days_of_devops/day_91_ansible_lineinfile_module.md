# Ansible lineinfile module

## Question

The Nautilus DevOps team want to install and set up a simple httpd web server on all app servers in Stratos DC. They also want to deploy a sample web page using Ansible. Therefore, write the required playbook to complete this task as per details mentioned below.

We already have an inventory file under `/home/thor/ansible` directory on jump host. Write a playbook `playbook.yml` under `/home/thor/ansible` directory on jump host itself. Using the playbook perform below given tasks:

1. Install `httpd` web server on all app servers, and make sure its `service` is up and running.

2. Create a file `/var/www/html/index.html` with content:

`This is a Nautilus sample file, created using Ansible!`

3. Using lineinfile Ansible module add some more content in `/var/www/html/index.html` file. Below is the content:

`Welcome to xFusionCorp Industries!`

Also make sure this new line is added at the top of the file.

4. The `/var/www/html/index.html` file's `user` and `group` owner should be `apache` on all app servers.

5. The `/var/www/html/index.html` file's `permissions` should be `0755` on all app servers.

## Answer

1. **Create the playbook.yml**

```yml
---
- name: App server setup
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

    - name: Create files
      ansible.builtin.copy:
        dest: "/var/www/html/index.html"
        content: "This is a Nautilus sample file, created using Ansible!"
        owner: apache
        group: apache
        mode: 0755

    - name: Add a header to the top of the file
      ansible.builtin.lineinfile:
        path: "/var/www/html/index.html"
        line: "Welcome to xFusionCorp Industries!"
        insertbefore: BOF
        state: present
```

3. **Run the playbook**

   - `ansible-playbook -i inventory playbook.yml`
