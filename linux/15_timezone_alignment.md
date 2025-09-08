# Timezone Alignment

## Question

In the daily standup, it was noted that the timezone settings across the Nautilus Application Servers in the Stratos Datacenter are inconsistent with the local datacenter's timezone, currently set to Asia/Manila.

Synchronize the timezone settings to match the local datacenter's timezone (Asia/Manila).

## Answer

I did it using `ansible`. Because, why not?

- Install ansible
```bash
sudo yum install -y ansible
```

- Create `inventory.ini` file
```inventory.ini
[nautlius_app_servers]
stapp01 ansible_user=tony ansible_ssh_pass=Ir0nM@n ansible_become_pass=Ir0nM@n
stapp02 ansible_user=steve ansible_ssh_pass=Am3ric@ ansible_become_pass=Am3ric@
stapp03 ansible_user=banner ansible_ssh_pass=BigGr33n ansible_become_pass=BigGr33n
```

- Create `playbook.yml` file
```playbook.yml
---
- name: Ensure timezone is Asia/Manila
  hosts: all
  become: true
  vars:
   new_timezone: 'Asia/Manila'
  tasks:
   - name: Set system timezone
     community.general.timezone:
       name: "{{ new_timezone }}"
```

- Customize Ansible behavior using the `ansible.cfg` file
```bash
vi /etc/ansible/ansible.cfg
```

- Add this line
```ansible.cfg
[defaults]
host_key_checking = False
```

- Run the ansible-playbook
```bash
ansible-playbook -i inventory.ini playbook.yml
```
```
```
