# temporary user setup with expiry

## Question

As part of the temporary assignment to the Nautilus project, a developer named james requires access for a limited duration. To ensure smooth access management, a temporary user account with an expiry date is needed. Here's what you need to do:

Create a user named james on App Server 1 in Stratos Datacenter. Set the expiry date to 2024-03-28, ensuring the user is created in lowercase as per standard protocol.

## Answer

login to App server 01 - stapp01 - tony
```bash
ssh tony@stapp01
```

Get the super user privilages
```bash
sudo su -
```

Create user with expiry date (-e)
```bash
useradd -e 2024-03-28 james
```

Check user added correctly with expiary date
```bash
chage -l james
```

## Additional Details

Additional commands with view user expiry details

View james's account expiary date
```bash
sudo chage -l james
```

## Brief Description About the Environment

**Temporary User Accounts**

In Linux, you can add a user with a temporary expiry date using the useradd command with the -e option. For example, useradd -e 2024-07-31 username creates a user whose account will expire on July 31, 2024. After this date, the user will not be able to log in.
