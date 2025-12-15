# Troubleshoot and Create Ansible Playbook

## Question

An Ansible playbook needs completion on the jump host, where a team member left off. Below are the details:

The inventory file `/home/thor/ansible/inventory` requires adjustments. The playbook must run on App Server 2 in Stratos DC. Update the inventory accordingly.

Create a playbook `/home/thor/ansible/playbook.yml`. Include a task to create an empty file `/tmp/file.txt` on App Server 2.

Note: Validation will run the playbook using the command `ansible-playbook -i inventory playbook.yml`. Ensure the playbook works without any additional arguments.

## Answer

1. **Troubleshoot inventory file**

   ```bash
   cd ansible

   vi inventory
   ```

   ```ini
   stapp02 ansible_host=172.238.16.12 ansible_user=steve ansible_ssh_common_args='-o StrictHostKeyChecking=no'
   ```

   - ip address needed to be change

2. **Create the playbook.yml**

   ```yaml
   ---
   - name: Create empty file on App Server 2
   hosts: stapp02
   tasks:
       - name: Create /tmp/file.txt
       file:
           path: /tmp/file.txt
           state: touch
   ```

3. **Setup passwordless play**

   ```bash
   ssh-keygen -t ed25519
   ssh-copy-id -i ~/.ssh/id_ed25519.pub steve@stapp02
   ```

4. **Run the playbook**

   - `ansible-playbook -i inventory playbook.yml`
