# Copy Data to App Servers using Ansible

## Question

The Nautilus DevOps team needs to copy data from the jump host to all application servers in Stratos DC using Ansible. Execute the task with the following details:

a. Create an inventory file `/home/thor/ansible/inventory` on `jump_host` and add all application servers as managed nodes.

b. Create a playbook `/home/thor/ansible/playbook.yml` on the jump host to copy the `/usr/src/security/index.html` file to all application servers, placing it at `/opt/security`.

## Answer

1. **Inventory file**

   ```bash
   cd ansible
   vi inventory
   ```

   ```ini
    [appservers]
    stapp01 ansible_host=172.16.238.10 ansible_user=tony ansible_ssh_common_args='-o StrictHostKeyChecking=no'
    stapp02 ansible_host=172.16.238.11 ansible_user=steve ansible_ssh_common_args='-o StrictHostKeyChecking=no'
    stapp03 ansible_host=172.16.238.12 ansible_user=banner ansible_ssh_common_args='-o StrictHostKeyChecking=no'
   ```

2. **Create the playbook.yml**

   ```yaml
    ---
    - name: Copy security index.html file to all app servers
    hosts: appservers
    gather_facts: no

    tasks:
        - name: Copy index.html from jump host to app servers
        ansible.builtin.copy:
            src: /usr/src/security/index.html
            dest: /opt/security/index.html
            owner: root
            group: root
            mode: '0644'
        become: yes
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
