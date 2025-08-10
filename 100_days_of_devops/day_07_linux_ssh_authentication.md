# Linux SSH authentication

## Question

The system admins team of xFusionCorp Industries has set up some scripts on jump host that run on regular intervals and perform operations on all app servers in Stratos Datacenter. To make these scripts work properly we need to make sure the thor user on jump host has password-less SSH access to all app servers through their respective sudo users (i.e tony for app server 1). Based on the requirements, perform the following:

Set up a password-less authentication from user thor on jump host to all app servers through their respective sudo users.

## Answer

Generate ssh key pair (rsa)
```bash
ssh-keygen -t rsa
```

Now your private and public keys are default saved in ~/.ssh/id_rsa ~/.ssh/id_rsa.pub.
we need to copy/store our ssh public key in all the app servers.

App server 01 - tony
```bash
ssh-copy-id tony@stapp01
```

Now login to stapp01 without password - to make sure whether our attempt is success.
```bash
ssh tony@stapp01
```

App server 02 - steve
```bash
ssh-copy-id steve@stapp02
```

Now login to stapp02 without password - to make sure whether our attempt is success.
```bash
ssh steve@stapp02
```

App server 03 - banner
```bash
ssh-copy-id banner@stapp03
```

Now login to stapp03 without password - to make sure whether our attempt is success.
```bash
ssh banner@stapp03
```

## Additional Details

Now your private and public keys are default saved in ~/.ssh/id_rsa ~/.ssh/id_rsa.pub.
we need to copy/store our ssh public key in all the app servers.
Provide Description

## Brief Description About the Environment

**Linux SSH authentication**

SSH authentication is the process of verifying a user's identity when connecting to a remote system using the Secure Shell (SSH) protocol. It commonly uses either password-based authentication, where the user enters a password, or key-based authentication, where a cryptographic key pair (private and public keys) is used. Key-based authentication is more secure and often preferred, as it eliminates the need to transmit passwords over the network.
