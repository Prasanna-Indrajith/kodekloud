# Jenkins Chained Builds

## Question

The DevOps team was looking for a solution where they want to restart Apache service on all app servers if the deployment goes fine on these servers in Stratos Datacenter. After having a discussion, they came up with a solution to use Jenkins chained builds so that they can use a downstream job for services which should only be triggered by the deployment job. So as per the requirements mentioned below configure the required Jenkins jobs.

Click on the Jenkins button on the top bar to access the `Jenkins UI`. Login using username `admin` and `Adm!n321` password.

Similarly you can access `Gitea UI` on port `8090` and username and password for Git is `sarah` and `Sarah_pass123` respectively. Under user sarah you will find a repository named web.

Apache is already installed and configured on all app server so no changes are needed there. The doc root `/var/www/html` on all these app servers is shared among the Storage server under `/var/www/html` directory.

1. Create a Jenkins job named `nautilus-app-deployment` and configure it to pull change from the master branch of web repository on Storage server under `/var/www/html` directory, which is already a local git repository tracking the origin web repository. Since `/var/www/html` on Storage server is a shared volume so changes should auto reflect on all apps.

2. Create another Jenkins job named manage-services and make it a downstream job for `nautilus-app-deployment` job. Things to take care about this job are:

a. This job should restart `httpd` service on all app servers.

b. Trigger this job only if the upstream job i.e `nautilus-app-deployment` is stable.

LB server is already configured. Click on the App button on the top bar to access the app. You should be able to see the latest changes you made. Please make sure the required content is loading on the main URL `https://<LBR-URL>` i.e there should not be a sub-directory like `https://<LBR-URL>/web` etc.

## Answer

1.  **Generate ssh key**

    - `ssh-keygen -t ed25519`

    - `ssh-copy-id natasha@ststor01`
    - `ssh natasha@ststor01`
    - `sudo echo "natasha    ALL=ALL    NOPASSWD:ALL" >> /etc/sudoers`

    - Do this for all the 3 applicatio servers
    - `ssh-copy-id <user>@<host>`
    - `ssh <user>@<host>`
      - `sudo echo "<user>    ALL=ALL    NOPASSWD:ALL" >> /etc/sudoers`

2.  **In jenkins Install the plugins**

    - `ssh agent` (This will not install at first hit. I have to install it 3 times.)

3.  **Create the global credential for sarah**

    - `Dashboard` > `Manage Jenkins` > `Credentials` > `System` > `Global credentials`
    - Add new Credential
      - kind : SSH username with private key
      - Username : `natasha`
      - password : `<natasha-passward>`
      - private key : Copy and past private key which is generated on `thor@jumphost` (~/.ssh/id_ed25519)

4.  **Create the job**

    - Name : `nautilus-app-deployment` (freestyle)
    - SSH agent

      - Credentials : `natasha`

    - Build Steps
      - Execute shell
        ```bash
            ssh -o StrictHostKeyChecking=no natasha@ststor01 "cd /var/www/html && git checkout master && git pull origin master"
        ```
    - Post build actions
      - Build other projects
        - Project to build : `manage-services`
        - Trigger only if build is stable

5.  **Create next the job**

    - Name : `manage-services` (freestyle)
    - Triggers

      - Build after other projects are built : `nautilus-app-deployment`
        - Trigger only build is stable

    - SSH agent

      - Credentials : `natasha`

    - Build Steps
      - Execute shell
        ```bash
            ssh -o StrictHostKeyChecking=no tony@stapp01 "sudo systemctl restart httpd"
            ssh -o StrictHostKeyChecking=no steve@stapp02 "sudo systemctl restart httpd"
            ssh -o StrictHostKeyChecking=no banner@stapp03 "sudo systemctl restart httpd"
        ```

There is lot know learn - https://youtu.be/PRaosJpqBso?si=XHAOwZWH4FWseaQh
IDK the creator. But Thank you!
