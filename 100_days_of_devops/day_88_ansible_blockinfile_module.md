# Ansible Blockinfile Module

## Question

The Nautilus DevOps team wants to `install` and set up a simple `httpd` web server on `all app servers` in Stratos DC. Additionally, they want to deploy a sample web page for now using Ansible only. Therefore, write the required playbook to complete this task. Find more details about the task below.

We already have an inventory file under `/home/thor/ansible` directory on jump host. Create a playbook.yml under `/home/thor/ansible` directory on jump host itself.

1. Using the playbook, install `httpd` web server on `all` app servers. Additionally, make sure its service **should up and running**.

2. Using `blockinfile` Ansible module add some content in `/var/www/html/index.html` file. Below is the content:

```
Welcome to XfusionCorp!

This is Nautilus sample file, created using Ansible!

Please do not modify this file manually!
```

3. The `/var/www/html/index.html` file's `user` and `group owner` should be `apache` on `all` app servers.

4. The `/var/www/html/index.html` file's permissions should be `0755` on `all` app servers.

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

    - name: Set permissions
      ansible.builtin.file:
        path: /var/www/html/index.html
        state: touch
        owner: apache
        group: apache
        mode: "0755"

    - name: Add context into /var/www/html/index.html
      ansible.builtin.blockinfile:
        path: /var/www/html/index.html
        block: |
          Welcome to XfusionCorp!

          This is  Nautilus sample file, created using Ansible!

          Please do not modify this file manually!
        state: present
```

2. **Run the playbook**

   - `ansible-playbook -i inventory playbook.yml`

## Additional Details

We can check the `inventory` file hosts

`ansible -m echo -i inventory all`
