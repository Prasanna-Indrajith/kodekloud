# Install and Configure PostgreSQL

## Question

The Nautilus application development team has shared that they are planning to deploy one newly developed application on Nautilus infra in Stratos DC. The application uses PostgreSQL database, so as a pre-requisite we need to set up PostgreSQL database server as per requirements shared below:

PostgreSQL database server is already installed on the Nautilus database server.

a. Create a database user kodekloud_aim and set its password to dCV3szSGNA.

b. Create a database kodekloud_db8 and grant full permissions to user kodekloud_aim on this database.

Note: Please do not try to restart the PostgreSQL server service.

## Answer

- SSH into the Database Server
- Login to the Postgres Database
  ```console
  psql -U postgres
  ```

- Create a Database User
  ```console
  CREATE USER kodekloud_aim WITH ENCRYPTED PASSWORD 'dCV3szSGNA';
  ```

- Create a Database
  ```console
  CREATE DATABASE kodekloud_db8;
  ```

- Give full access to the user on the database
  ```console
  GRANT ALL PRIVILEGES ON DATABASE kodekloud_db8 TO kodekloud_aim;
  ```

- Exit server

## Brief Description About the Environment

**PostgreSQL why?**

PostgreSQL is an advanced, open-source relational database management system (RDBMS) widely chosen for its reliability, data integrity, and powerful features. It's often referred to as "the world's most advanced open source relational database" for good reason.
