# Install Ansible

## Question

During the weekly meeting, the Nautilus DevOps team discussed about the automation and configuration management solutions that they want to implement. While considering several options, the team has decided to go with Ansible for now due to its simple setup and minimal pre-requisites. The team wanted to start testing using Ansible, so they have decided to use jump host as an Ansible controller to test different kind of tasks on rest of the servers.

Install ansible version 4.7.0 on Jump host using pip3 only. Make sure Ansible binary is available globally on this system, i.e all users on this system are able to run Ansible commands.

## Answer

Update packages
```bash
sudo yum update -y
```

Install Ansible for all users (by run with `sudo` and remove nomal installation flag `--user`)
```bash
sudo pip3 install ansible==4.10.0
```

## Additional Details

Also we can install Ansible in many different ways.

## Brief Description About the Environment

**What is Ansible ?**

Ansible is an open-source automation tool used for configuration management, application deployment, and task automation. It allows you to define the desired state of your systems and applications using simple, human-readable YAML files called playbooks.

Key points about Ansible:

- Agentless: No software needs to be installed on managed nodes; it uses SSH for communication.
- Declarative: You describe what you want, not how to do it.
- Idempotent: Running the same playbook multiple times produces the same result.
- Scalable: Can manage a few or thousands of servers.
- Extensible: Supports modules for various tasks (e.g., installing packages, copying files).

Typical use cases include provisioning servers, deploying applications, and automating repetitive IT tasks.



