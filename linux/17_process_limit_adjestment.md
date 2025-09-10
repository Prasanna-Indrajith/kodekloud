# Process Limit Adjesment

## Question

In the Stratos Datacenter, our Storage server is encountering performance degradation due to excessive processes held by the nfsuser user. To mitigate this issue, we need to enforce limitations on its maximum processes. Please set the maximum process limits as specified below:

a. Set the soft limit to 1027
b. Set the hard limit to 2027

## Answer

- ssh into Storage Server
```bash
ssh natasha@ststor01

sudo su -
```

- Edit the boundries
```bash
vi /etc/security/limits.conf
```

- Add these conditions to end of the specified
```conf
nfsuser soft  nproc 1027
nfsuser hard  nproc 2027
```

## Brief Description About the Environment

**Linux boundries**


