# Jenkins Database Backup Job

## Question

There is a requirement to create a Jenkins job to automate the database backup. Below you can find more details to accomplish this task:

Click on the Jenkins button on the top bar to access the Jenkins UI. Login using username `admin` and password `Adm!n321`.

Create a Jenkins job named `database-backup`.

Configure it to take a database dump of the `kodekloud_db01` database present on the `Database server` in Stratos Datacenter, the database user is `kodekloud_roy` and password is asdfgdsd.

The dump should be named in `db\_$(date +%F).sql` format, where `date +%F` is the current date.

Copy the db\_$(date +%F).sql dump to the Backup Server under location `/home/clint/db_backups`.

Further, schedule this job to run periodically at `_/10 _ \* \* \*` (please use this exact schedule format).

Note:

You might need to install some plugins and restart Jenkins service. So, we recommend clicking on Restart Jenkins when installation is complete and no jobs are running on plugin installation/update page i.e update centre. Also, Jenkins UI sometimes gets stuck when Jenkins service restarts in the back end. In this case please make sure to refresh the UI page.

Please make sure to define you cron expression like this `_/10 _ \* \* \*` (this is just an example to run job every 10 minutes).

For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.

## Answer

### **Configuration Steps**

1.  **Access Jenkins UI and Log In**

    - Logged into the Jenkins UI using `admin` credentials.

2.  **Establish Server Connections**

    - Logged into the **Database Server (`stdb01`)** as user `Peter`.
    - Logged into the **Backup Server (`stbkp01`)** as user `Clint`.
    - Became `root` on both servers.

3.  **Verify Environment**

    - Confirmed the backup directory (`/home/clint/database_db_backups`) on the backup server was empty.
    - Verified the existence of the `kodekloud_db01` database on the DB server.

4.  **Install Jenkins Plugins**

    - Installed the necessary plugins: **SSH**, **SSH Credentials**, and **Publish Over SSH**.

5.  **Add Server Credentials**

    - Added **Global Credentials** for:
      - **DB Server:** Username: `Peter`
      - **Backup Server:** Username: `Clint`

6.  **Configure SSH Remote Hosts**

    - Configured **Publish over SSH** settings for:
      - **DB Server:** Hostname: `stdb01`, Credentials: `Peter`
      - **Backup Server:** Hostname: `stbkp01`, Credentials: `Clint`
    - Tested connections successfully.

7.  **Create Jenkins Job**

    - Created a new **Freestyle project** named `database-backup`.

8.  **Set Build Schedule**

    - Configured the job to **Build periodically** every 10 minutes using the cron expression: `*/10 * * * *`.

9.  **Add Build Step (Execute Remote Script)**

    - Added an "Execute script on remote host using SSH" step, targeting the **DB server** (`stdb01`).
    - The script performs the database dump and secure file copy:

    <!-- end list -->

    ```bash
    mysqldump -u kodekloud_roy -p'asdfgdsd' kodekloud_db01 > db_dow_$(date +%F).sql
    sshpass -p 'H@wk3y3' scp -p -o StrictHostKeyChecking=no db_dow_$(date +%F).sql clint@stbkp01:/home/clint/database_db_backups/
    ```

---

### **Verification**

10. **Trigger and Verify Build**

    - Manually triggered the job ("Build Now") and verified successful completion in the **Console Output**.
    - Confirmed the presence of the dumped SQL file on the backup server (`stbkp01`) in the `/home/clint/database_db_backups/` directory.

11. **Validate Task Completion**

    - The work was successfully validated by the system.
