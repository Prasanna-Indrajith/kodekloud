# Create Ansible Inventory for App Server Testing

## Question

The Nautilus DevOps team is testing Ansible playbooks on various servers within their stack. They've placed some playbooks under /home/thor/playbook/ directory on the jump host and now intend to test them on app server 3 in Stratos DC. However, an inventory file needs creation for Ansible to connect to the respective app. Here are the requirements:

a. Create an ini type Ansible inventory file /home/thor/playbook/inventory on jump host.

b. Include App Server 3 in this inventory along with necessary variables for proper functionality.

c. Ensure the inventory hostname corresponds to the server name as per the wiki, for example stapp01 for app server 1 in Stratos DC.

Note: Validation will execute the playbook using the command ansible-playbook -i inventory playbook.yml. Ensure the playbook functions properly without any extra arguments.

## Answer


```bash
cat playbook.yml

vi inventory

    [all] # group name
    stapp03 ansible_host=172.16.238.12 ansible_user=banner -o StrictHostKeyChecking=no
    :wq

```

Run the playbook
```bash
ansible-playbook -i inventory playbook.yml --ask-pass # this is for runtime password asking. Other wise we can setup secure ssh connection and and login using password less.

PLAY RECAP **********************************************************************
stapp03                    : ok=3    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

```

**What does ansible inventory file ?**


An Ansible inventory file defines the hosts and groups of hosts that Ansible manages. It lists the target machines for automation tasks and can specify variables for them.

**Formats:**
- INI (default):  
  ```
  [web]
  server1.example.com
  server2.example.com

  [db]
  db1.example.com
  ```
- YAML (for dynamic inventories or with `ansible-inventory`):  
  ```yaml
  all:
    hosts:
      server1.example.com:
      server2.example.com:
    children:
      web:
        hosts:
          server1.example.com:
      db:
        hosts:
          db1.example.com:
  ```

**Location:**  
Default is `/etc/ansible/hosts`, but you can specify with `-i <inventory_file>`.

**Purpose:**  
It tells Ansible which machines to connect to and how to group them for playbooks and ad-hoc commands.

