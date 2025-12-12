# Jenkins Scheduled Jobs

## Question

The devops team of xFusionCorp Industries is working on to setup centralised logging management system to maintain and analyse server logs easily. Since it will take some time to implement, they wanted to gather some server logs on a regular basis. At least one of the app servers is having issues with the Apache server. The team needs Apache logs so that they can identify and troubleshoot the issues easily if they arise. So they decided to create a Jenkins job to collect logs from the server. Please create/configure a Jenkins job as per details mentioned below:

Click on the Jenkins button on the top bar to access the Jenkins UI. Login using username admin and password Adm!n321

1. Create a Jenkins jobs named `copy-logs`.

2. Configure it to periodically build every 10 minutes to copy the Apache logs (both `access_log` and `error_logs`) from App Server 1 (from default logs location) to location `/usr/src/dba` on Storage Server.

Note:

1. You might need to install some plugins and restart Jenkins service. So, we recommend clicking on Restart Jenkins when installation is complete and no jobs are running on plugin installation/update page i.e update centre. Also, Jenkins UI sometimes gets stuck when Jenkins service restarts in the back end. In this case please make sure to refresh the UI page.

2. Please make sure to define you cron expression like this `_/10 _ \* \* \*` (this is just an example to run job every 10 minutes).

3. For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.

## Answer

Steps,

1. Accessed Jenkins UI and Logged In

2. Established SSH Connections to Servers (`ststor01, stapp01`)

3. Verified Log Paths : The default Apache log location (/var/log/httpd) on the app server and the destination directory (/usr/src/dba) on the storage server

4. Installed Required Jenkins Plugins : `SSH Plugin, SSH Credentials Plugin, Publish Over SSH`

5. Restarted Jenkins Service

6. Added Server Credentials to Jenkins : SSH credentials (username and password) for both the app01 (Tony user) and st01 (Natasha user) servers were added to Jenkins's global credentials.

7. Configured SSH Remote Hosts : Jenkins was configured to recognize app01 and st01 as remote SSH hosts, specifying their hostnames and associated credentials.

8. Created a Jenkins Job : A new freestyle project named `copy-logs` was created.

9. Configured Periodic Build Schedule : The copy-logs job was set to build periodically every 7 minutes using the cron expression `_/10 _ \* \* \*`.

10. Added Build Step to Execute Script on Remote Host : An "Execute shell script on remote host using SSH" build step was added. The script used sshpass and scp to copy all files from `/var/log/httpd/` on the app server to `/usr/src/dba/` on the storage server.

11. Troubleshooted and Corrected Script Errors : Initial build failures due to incorrect pathing and sshpass syntax in the script were identified and fixed.

12. Triggered Build and Verified Logs : After corrections, the job was successfully built, and the presence of access_log and error_log in the `/usr/src/dba/` directory on the storage server was confirmed.

13. Validated Task Completion : The task was validated as complete by the system.

```bash
sshpass -p 'Bl@kW' scp -p  -o  StrictHostKeyChecking=no /var/log/httpd/* natasha@ststor01:/usr/src/dba
```
