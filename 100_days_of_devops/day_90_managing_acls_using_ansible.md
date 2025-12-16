# Managing ACLs Using Ansible

## Question

There are some files that need to be created on all app servers in Stratos DC. The Nautilus DevOps team want these files to be owned by user root only however, they also want that the app specific user to have a set of permissions on these files. All tasks must be done using Ansible only, so they need to create a playbook. Below you can find more information about the task.

1. Create a playbook named `playbook.yml` under `/home/thor/ansible` directory on jump host, an inventory file is already present under `/home/thor/ansible` directory on Jump Server itself.

2. Create an empty file `blog.txt` under `/opt/security/` directory on `app server 1`. Set some acl properties for this file. Using acl provide `read` '(r)' permissions to group `tony` (i.e `entity` is `tony` and `etype` is `group`).

3. Create an empty file `story.txt` under `/opt/security/` directory on `app server 2`. Set some acl properties for this file. Using acl provide `read + write` '(rw)' permissions to user `steve` (i.e `entity` is `steve` and `etype` is `user`).

4. Create an empty file `media.txt` under `/opt/security/` on `app server 3`. Set some acl properties for this file. Using acl provide `read + write` '(rw)' permissions to group `banner` (i.e `entity` is `banner` and `etype` is `group`).

## Answer

1. **Update the inventory file**

```ini
stapp01 ansible_host=172.16.238.10 ansible_ssh_pass=Ir0nM@n ansible_user=tony file_name=blog permissions=r type=group
stapp02 ansible_host=172.16.238.11 ansible_ssh_pass=Am3ric@ ansible_user=steve file_name=story permissions=rw type=user
stapp03 ansible_host=172.16.238.12 ansible_ssh_pass=BigGr33n ansible_user=banner file_name=media permissions=rw type=group
```

2. **Create the playbook.yml**

```yml
---
- name: ACL setup
  hosts: all
  become: yes
  tasks:
    - name: Create files
      ansible.builtin.file:
        path: "/opt/security/{{file_name}}.txt"
        state: touch

    - name: Grant user access
      ansible.posix.acl:
        path: "/opt/security/{{file_name}}.txt"
        entity: "{{ ansible_user }}"
        etype: "{{ type }}"
        permissions: "{{ permissions }}"
        state: present
```

3. **Run the playbook**

   - `ansible-playbook -i inventory playbook.yml`
