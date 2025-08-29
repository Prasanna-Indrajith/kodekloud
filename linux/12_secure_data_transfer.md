# Secure Data Transfer

## Question

A Nautilus developer has stored confidential data on the jump host within Stratos DC. To ensure security and compliance, this data must be transferred to one of the app servers. Given developers lack direct access to these servers, the system admin team has been enlisted for assistance.

Copy /tmp/nautilus.txt.gpg file from jump server to App Server 3 placing it in the directory /home/web.

## Answer

- Just copy the file into App Server 3 using `scp`
```bash
scp /tmp/nautilus.txt.gpg banner@stapp03:/home/web
```

- ssh into App Server 3 and check if that file is available in that location.

## Brief Description About the Environment

**SCP in linux?**

SCP (Secure Copy Protocol) allows users to securely transfer files between systems. Because it uses SSH, all transferred data is encrypted, making it a safe choice for transmitting sensitive files.
