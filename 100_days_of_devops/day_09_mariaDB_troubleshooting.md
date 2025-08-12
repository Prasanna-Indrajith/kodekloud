# MariaDB Troubleshooting

## Question

There is a critical issue going on with the Nautilus application in Stratos DC. The production support team identified that the application is unable to connect to the database.
After digging into the issue, the team found that mariadb service is down on the database server.

Look into the issue and fix the same.

## Answer

SSH into Database server and see what's going on
```bash
ssh peter@stdb01
sudo systemctl start mariadb
```

Get the super user previlages
```bash
sudo sh -
```

In those errors I got this line of error `Aug 12 21:09:32 stdb01.stratos.xfusioncorp.com mariadb-prepare-db-dir[6610]: Make sure the /var/lib/mysql is empty before running mariadb-prepare-db-dir.`

After I check the mysql directory for is there any problem there
```bash
ls /var/lib/mysql
```

There is no such a directory there in my case. So I create the file and gave the ownership and proper execution permission like this.
```bash
chown -R mysql:mysql /var/lib/mysql

chmod 755 /var/lib/mysql
```

Then start the mariadb
```bash
systemctl start mariadb
```

## Brief Description About the Environment

**What is MariaDB**

MariaDB is an open-source relational database management system (RDBMS), designed as a drop-in replacement for MySQL. It is highly compatible with MySQL, offers improved performance, and includes additional features like better storage engines (e.g., Aria, XtraDB) and enhanced security. MariaDB is widely used for web applications, data storage, and managing structured data.
