# Jenkins Multistage Pipeline

## Question

The development team of xFusionCorp Industries is working on to develop a new static website and they are planning to deploy the same on Nautilus App Servers using Jenkins pipeline. They have shared their requirements with the DevOps team and accordingly we need to create a Jenkins pipeline job. Please find below more details about the task:

Click on the Jenkins button on the top bar to access the Jenkins UI. Login using username admin and password Adm!n321.

Similarly, click on the Gitea button on the top bar to access the Gitea UI. Login using username sarah and password Sarah_pass123.

There is a repository named sarah/web in Gitea that is already cloned on Storage server under `/var/www/html` directory.

Update the content of the file `index.html` under the same repository to `Welcome to xFusionCorp Industries` and push the changes to the origin into the `master` branch.

Apache is already installed on all app Servers its running on port 8080.

Create a Jenkins pipeline job named `deploy-job` (it must not be a Multibranch pipeline job) and pipeline should have two stages Deploy and Test ( names are case sensitive ). Configure these stages as per details mentioned below.

a. The Deploy stage should deploy the code from web repository under `/var/www/html` on the Storage Server, as this location is already mounted to the document root `/var/www/html` of all app servers.

b. The Test stage should just test if the app is working fine and website is accessible. Its up to you how you design this stage to test it out, you can simply add a `curl` command as well to run a `curl` against the LBR URL (`http://stlb01:8091`) to see if the website is working or not. Make sure this stage fails in case the website/app is not working or if the Deploy stage fails.

## Answer

1.  **Make changes to index.html**

    ```bash
    ssh natasha@ststor01

    // Go into relevent directory
    cd /var/www/html

    // Edit the file
    echo "Welcome to xFusionCorp Industries" > index.html

    // Push changes into remote repository
    git add index.html
    git commit -m "content updated"
    git push
    ```

2.  **In jenkins Install the plugins**

    - `pipeline` (This will not install at first hit. I have to install it 3 times.)

3.  **Create Pipeline**

    - Name : `deploy-job`
    - Script :

    ```
    pipeline {
        agent any

        stages {
            stage('Deploy') {
                steps {
                    script {
                        sh "sshpass -p 'Bl@kW' ssh -o StrictHostKeyChecking=no natasha@ststor01 'cd /var/www/html && git checkout master && git pull origin master'"
                    }
                }
            }

            stage('Test') {
                steps {
                    script {
                        def response_code = sh(script: 'curl -o /dev/null -s -w "%{http_code}" http://stlb01:8091', returnStdout: true)
                        if (response_code != '200') {
                            error("Deployment not success! Error code : ${response_code}")
                        } else {
                            echo "Deployment success!"
                        }
                    }
                }
            }
        }
    }

    ```

4.  **Last step**
    - Verify the pipeline is work fine by `console output`

There is lot know learn - https://youtu.be/1he0M7mWZyQ?si=D0PWpjmYFnzGx1qG
IDK the creator. But Thank you!
