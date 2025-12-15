# Jenkins Deployment Job

## Question

The Nautilus development team had a meeting with the DevOps team where they discussed automating the deployment of one of their apps using Jenkins (the one in Stratos Datacenter). They want to auto deploy the new changes in case any developer pushes to the repository. As per the requirements mentioned below configure the required Jenkins job.

Click on the `Jenkins` button on the top bar to access the Jenkins UI. Login using username `admin` and `Adm!n321` password.

Similarly, you can access the `Gitea UI` using `Gitea` button, username and password for Git is `sarah` and `Sarah_pass123` respectively. Under user `sarah` you will find a repository named `web` that is already cloned on the Storage server under sarah's home. `sarah` is a developer who is working on this repository.

1. Install `httpd` (whatever version is available in the yum repo by default) and configure it to serve on port `8080` on All app servers. You can make it part of your Jenkins job or you can do this step manually on all app servers.

2. Create a Jenkins job named `nautilus-app-deployment` and configure it in a way so that if anyone pushes any new change to the origin repository in `master` branch, the job should auto build and deploy the latest code on the Storage server under `/var/www/html` directory. Since `/var/www/html` on `Storage server` is shared among all apps.
   Before deployment, ensure that the ownership of the `/var/www/html` directory is set to user `sarah`, so that Jenkins can successfully deploy files to that directory.

3. SSH into `Storage Server` using `sarah` user credentials mentioned above. Under `sarah` user's home you will find a cloned Git repository named `web`. Under this repository there is an` index.html` file, update its content to `Welcome to the xFusionCorp Industries`, then push the changes to the `origin` into `master` branch. This push must trigger your Jenkins job and the latest changes must be deployed on the servers, also make sure it deploys the entire repository content not only `index.html` file.

Click on the App button on the top bar to access the app, you should be able to see the latest changes you deployed. Please make sure the required content is loading on the main URL `https://<LBR-URL>` i.e there should not be any sub-directory like `https://<LBR-URL>/web` etc.

## Answer

1.  **Access Jenkins UI and SSH to Storage Server**

    - Logged into the Jenkins UI using `admin` credentials.
    - Logged into Storage server using user `natasha`.
    - Logged into all the `app servers`.

2.  **Works to done in Storage server as user natasha**

    ```bash
    //Install java
    sudo yum install -y java-17-openjdk screen

    // Give permissions to sarah
    sudo chown -R sarah:sarah /var/www/html
    ```

3.  **Works to done in all Application Servers**

    - On all three app servers (`stapp01`, `stapp02`, `stapp03`)

    ```bash
    // Install and configure apache
    sudo yum install -y httpd && sudo sed -i 's/Listen 80/listen 8080/' /etc/httpd/conf/httpd.conf && sudo systemctl enable --now httpd && curl localhost:8080
    ```

4.  **Install plugins on Jenkins**

    - `git` (This will not install at first hit. I have to install it 3 times.)

5.  **Change the jenkin default URL**

    - `Dashboard` > `Manage Jenkins` > `System`
    - Jenkin URL : http://jenkins:8080

6.  **Create the node**

    - `Dashboard` > `Manage Jenkins` > `Nodes`
      - Name : `Storage Server`
      - Labels : `ststor01`
      - Remote root directory : `/var/www/html`
      - Launch method : Keep the default

    After save go into newly created node and then execute the commands given in that page one by one with root privilages in `storage server` terminal as user `sarah`. Now node will build successfully.

7.  **Create the global credential for sarah**

    - `Dashboard` > `Manage Jenkins` > `Credentials` > `System` > `Global credentials`
    - Add new Credential
      - Username : `sarah`
      - password : `<given-passward>`
      - id : `sarah`

8.  **Create the job**

    - Name : `nautilus-app-deployment` (freestyle)
    - [x] restrict where this project can be run
      - `ststor01`
    - Source code management

      - Select `Git`
      - Repository url : can be found of `Gitea` profile of sarah
      - Credentials : select previously created `sarah`

    - Triggers

      - Poll SCM : `* * * * *` (run for every minute)

    - Build Steps
      - Execute shell
        ```bash
        cd /var/www/html
        git pull
        ```

9.  **Works to done in Storage server as user sarah**

    ```bash
    screen -S jenkins

    //<'here execute the commands get while creating node(step 07)'>
    ```

    - Now change the content of `~/web/index.html` to they given

10. **Is it works fine?**

    - Now we need to ensure all the things working correctly.

    1.  Make sure `Node` is running
    2.  Make sure `job` is running
    3.  Goto `App` on kodekloud Dashboard
        - Check it shows the content you entered in `step 9`
    4.  SSH into `App Servers` and make sure site is accessible through `PORT 8080`
        - `curl localhost:8080`

There is lot know learn - https://youtu.be/4W7kNQKCBPg?si=ugplJZTTXvD-YZGj
IDK the creator. But Thank you!
