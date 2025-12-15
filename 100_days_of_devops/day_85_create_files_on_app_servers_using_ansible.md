# Create Files on App Servers using Ansible

## Question

The Nautilus DevOps team is testing various Ansible modules on servers in Stratos DC. They're currently focusing on file creation on remote hosts using Ansible. Here are the details:

a. Create an inventory file `~/playbook/inventory` on jump host and include all app servers.

b. Create a playbook `~/playbook/playbook.yml` to create a blank file `/tmp/code.txt` on all app servers.

c. Set the permissions of the `/tmp/code.txt` file to `0644`.

d. Ensure the `user/group owner` of the `/tmp/code.txt` file is `tony` on app server 1, `steve` on app server 2 and `banner` on app server 3.

## Answer

1. **Inventory file**

   ```bash
   cd playbook
   vi inventory
   ```

   ```ini
    [appservers]
    stapp01 ansible_host=172.16.238.10 ansible_user=tony ansible_ssh_common_args='-o StrictHostKeyChecking=no' file_owner=tony file_group=tony
    stapp02 ansible_host=172.16.238.11 ansible_user=steve ansible_ssh_common_args='-o StrictHostKeyChecking=no' file_owner=steve file_group=steve
    stapp03 ansible_host=172.16.238.12 ansible_user=banner ansible_ssh_common_args='-o StrictHostKeyChecking=no'  file_owner=banner file_group=banner
   ```

2. **Create the playbook.yml**

   ```yaml
    ---
    - name: Create /tmp/code.txt and set ownership
    hosts: appservers
    gather_facts: no
    become: yes

    tasks:
        - name: Create /tmp/code.txt and set ownership
        ansible.builtin.file:
            path: /tmp/code.txt
            state: touch
            owner: "{{ file_owner }}"
            group: "{{ file_group }}"
            mode: '0644'

   ```

3. **Setup passwordless play**

   ```bash
    ssh-keygen -t ed25519

    ssh-copy-id -i ~/.ssh/id_ed25519.pub tony@stapp01
    ssh-copy-id -i ~/.ssh/id_ed25519.pub steve@stapp02
    ssh-copy-id -i ~/.ssh/id_ed25519.pub banner@stapp03
   ```

4. **Run the playbook**

   - `ansible-playbook -i inventory playbook.yml`
