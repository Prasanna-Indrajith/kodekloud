# Using Ansible Conditionals

## Question

The Nautilus DevOps team had a discussion about, how they can train different team members to use Ansible for different automation tasks. There are numerous ways to perform a particular task using Ansible, but we want to utilize each aspect that Ansible offers. The team wants to utilise Ansible's conditionals to perform the following task:

An `inventory` file is already placed under `/home/thor/ansible` directory on jump host, with all the Stratos DC app servers included.

Create a playbook `/home/thor/ansible/playbook.yml` and make sure to use Ansible's `when` conditionals statements to perform the below given tasks.

1. Copy `blog.txt` file present under `/usr/src/itadmin` directory on jump host to App Server 1 under `/opt/itadmin` directory. Its user and `group owner` must be user `tony` and its `permissions` must be `0655` .

2. Copy `story.txt` file present under `/usr/src/itadmin` directory on jump host to App Server 2 under `/opt/itadmin` directory. Its user and `group owner` must be user `steve` and its `permissions` must be `0655` .

3. Copy `media.txt` file present under `/usr/src/itadmin` directory on jump host to App Server 3 under `/opt/itadmin` directory. Its user and `group owner` must be user `banner` and its `permissions` must be `0655`.

NOTE: You can use `ansible_nodename` variable from gathered facts with when condition. Additionally, please make sure you are running the play for all hosts i.e use `- hosts: all`.

## Answer

1. **Create the playbook.yml**

```yml
---
- name: Conditional File Copy for App Servers
  hosts: all
  become: yes
  tasks:
    - name: Ensure destination directory exists
      ansible.builtin.file:
        path: /opt/itadmin
        state: directory

    - name: Copy blog.txt to App Server 1
      ansible.builtin.copy:
        src: /usr/src/itadmin/blog.txt
        dest: /opt/itadmin/blog.txt
        owner: tony
        group: tony
        mode: "0655"
      when: inventory_hostname == "stapp01"

    - name: Copy story.txt to App Server 2
      ansible.builtin.copy:
        src: /usr/src/itadmin/story.txt
        dest: /opt/itadmin/story.txt
        owner: steve
        group: steve
        mode: "0655"
      when: inventory_hostname == "stapp02"

    - name: Copy media.txt to App Server 3
      ansible.builtin.copy:
        src: /usr/src/itadmin/media.txt
        dest: /opt/itadmin/media.txt
        owner: banner
        group: banner
        mode: "0655"
      when: inventory_hostname == "stapp03"
```

3. **Run the playbook**

   - `ansible-playbook -i inventory playbook.yml`
