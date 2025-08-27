# 

## Question

The DevOps team established a new Git repository last week, which remains unused at present. However, the Nautilus application development team now requires a copy of this repository on the Storage Server in the Stratos DC. Follow the provided details to clone the repository:

The repository to be cloned is located at /opt/official.git
Clone this Git repository to the /usr/src/kodekloudrepos directory. Ensure no modifications are made to the repository during the cloning process.

## Answer

- ssh into Storage Server
```bash
ssh natasha@ststor01

sudo su -
```

- cd `/usr/src/kodekloudrepos/` and do the cloning
```bash
cd /usr/src/kodekloudrepos/

git clone /opt/official.git
```

- Check the clone
```bash
ls -la
```

## Additional Details

Simple clone process.
