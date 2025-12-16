# Ansible ping module

## Question

The Nautilus DevOps team is planning to test several Ansible playbooks on different app servers in Stratos DC. Before that, some pre-requisites must be met. Essentially, the team needs to set up a **password-less SSH connection** between Ansible controller and Ansible managed nodes. One of the tickets is assigned to you; please complete the task as per details mentioned below:

a. `Jump host` is our Ansible controller, and we are going to run Ansible playbooks through `thor` user from jump host.

b. There is an inventory file `/home/thor/ansible/inventory` on jump host. Using that inventory file test Ansible ping from jump host to App Server 1, make sure ping works.

## Answer

1. **Ansible configuration**

   ```bash
   cd ansible

   echo "[defaults]\nANSIBLE_HOST_KEY_CHECKING=False" > ansible.cfg
   ```

2. **Inventory file changes**

   ```ini
   stapp01 ansible_host=172.16.238.10 ansible_user=tony
   stapp02 ansible_host=172.16.238.11 ansible_user=steve
   stapp03 ansible_host=172.16.238.12 ansible_user=banner
   ```

3. **Playbook.yml**

   ```yaml
   ---
   - name: Built-in ping
     hosts: stapp03
     gather_facts: no

     tasks:
       - name: Ping to stapp03
         ansible.builtin.ping:
   ```

4. **Setup password-less play**

   ```bash
    ssh-keygen -t ed25519

    ssh-copy-id -i ~/.ssh/id_ed25519.pub tony@stapp01
    ssh-copy-id -i ~/.ssh/id_ed25519.pub steve@stapp02
    ssh-copy-id -i ~/.ssh/id_ed25519.pub banner@stapp03
   ```

5. **Run the playbook**

   - `ansible-playbook -i inventory playbook.yml`
