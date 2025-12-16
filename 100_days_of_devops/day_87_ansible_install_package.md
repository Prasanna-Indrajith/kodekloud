# Ansible Install Package

## Question

The Nautilus Application development team wanted to test some applications on app servers in Stratos Datacenter. They shared some pre-requisites with the DevOps team, and packages need to be installed on `app servers`. Since we are already using Ansible for automating such tasks, please perform this task using Ansible as per details mentioned below:

Create an inventory file `/home/thor/playbook/inventory` on jump host and add all app servers in it.

Create an Ansible playbook `/home/thor/playbook/playbook.yml` to install `wget` package on all app servers using **Ansible yum module**.

Make sure user thor should be able to run the playbook on jump host.

## Answer

1. **Ansible configuration**

   ```bash
   cd ansible

   echo "[defaults]\nANSIBLE_HOST_KEY_CHECKING=False" > ansible.cfg
   ```

2. **Inventory file**

   ```ini
   stapp01 ansible_host=172.16.238.10 ansible_user=tony
   stapp02 ansible_host=172.16.238.11 ansible_user=steve
   stapp03 ansible_host=172.16.238.12 ansible_user=banner
   ```

3. **Playbook.yml**

   ```yaml
    ---
    - name: Install wget
      hosts: all
      become: yes

      tasks:
          - name: install wget
              ansible.builtin.yum:
                name: wget
                state: present
   ```

4. **Setup password-less play**

   ```bash
    ssh-keygen -t ed25519 && \
    ssh-copy-id -i ~/.ssh/id_ed25519.pub tony@stapp01 && \
    ssh-copy-id -i ~/.ssh/id_ed25519.pub steve@stapp02 && \
    ssh-copy-id -i ~/.ssh/id_ed25519.pub banner@stapp03
   ```

5. **Run the playbook**

   - `ansible-playbook -i inventory playbook.yml`
