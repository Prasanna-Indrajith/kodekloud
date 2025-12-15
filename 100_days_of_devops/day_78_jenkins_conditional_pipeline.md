# Jenkins Conditional Pipeline

## Question

The development team of xFusionCorp Industries is working on to develop a new static website and they are planning to deploy the same on Nautilus App Servers using Jenkins pipeline. They have shared their requirements with the DevOps team and accordingly we need to create a Jenkins pipeline job. Please find below more details about the task:

Click on the Jenkins button on the top bar to access the Jenkins UI. Login using username `admin` and password `Adm!n321`.

Similarly, click on the Gitea button on the top bar to access the Gitea UI. Login using username `sarah` and password `Sarah_pass123`. There under user sarah you will find a repository named `web_app` that is already cloned on Storage server under `/var/www/html`. sarah is a developer who is working on this repository.

Add a slave node named Storage Server. It should be labeled as ststor01 and its remote root directory should be `/var/www/html`.

We have already cloned repository on `Storage Server` under `/var/www/html`.

Apache is already installed on all app Servers its running on port `8080`.

Create a Jenkins pipeline job named `nautilus-webapp-job` (it must not be a Multibranch pipeline) and configure it to:

Add a string parameter named `BRANCH`.

It should conditionally deploy the code from `web_app` repository under `/var/www/html` on Storage Server, as this location is already mounted to the document root `/var/www/html` of app servers. The pipeline should have a single stage named Deploy ( which is case sensitive ) to accomplish the deployment.

The pipeline should be conditional, if the value `master` is passed to the `BRANCH` parameter then it must deploy the `master` branch, on the other hand if the value `feature` is passed to the `BRANCH` parameter then it must deploy the `feature` branch.

LB server is already configured. You should be able to see the latest changes you made by clicking on the App button. Please make sure the required content is loading on the main URL `https://<LBR-URL>` i.e there should not be a sub-directory like `https://<LBR-URL>/web_app` etc.

## Answer

1.  **Access Jenkins UI and SSH to Storage Server**

    - Logged into the Jenkins UI using `admin` credentials.
    - Logged into Storage server using user `natasha`.

2.  **Works to done in Storage server**

    ```bash
    sudo yum install java-17-openjdk screen -y

    screen -S jenkin

    cd /var/www/html
    git remote -v
    git config --global --add safe.directory /var/www/html
    sudo !!
    ```

3.  **Install plugins**

    - `pipeline` (This will not install at first hit. I have to install it 3 times.)

4.  **Change the jenkin default URL**

    - `Dashboard` > `Manage Jenkins` > `System`
    - Jenkin URL : http://jenkins:8080

5.  **Create the slave node**

    - `Dashboard` > `Manage Jenkins` > `Nodes`
      - Name : `Storage Server`
      - Labels : `ststor01`
      - Remote root directory : `/var/www/html`
      - Launch method : Keep the default

    After save go into newly created node and then execute the commands given in that page one by one with root privilages in `storage server` terminal. Now node will build successfully.

6.  **Create the pipeline**

    - `Dashboard` > `New item` (pipeline)
    - Pipeline script

      ````jenkins
      pipeline {
        agent {label 'ststor01'}
        parameters {
            string(name: 'BRANCH', defaultValue: 'mater', description: 'Branch name')
        }
        stages {
            stage('Deploy') {
                script {
                    steps {
                        echo "Starting deployement"
                        if (params.BRANCH == 'master') {
                            echo 'Deploy on master branch'
                            sh ```
                                cd /var/www/html
                                git fetch origin
                                git checkhout master
                                git pull origin master
                            ```
                        } else if (params.BRANCH == 'feature') {
                            echo 'Deploy on feature branch'
                            sh ```
                                cd /var/www/html
                                git fetch origin
                                git checkhout feature
                                git pull origin feature
                            ```
                        } else {
                            echo 'Nothing to do'
                        }

                    }
                }
            }
        }
      }
      ````

    - Save and build it now

## Additional Details

There is lot know learn - https://www.youtube.com/watch?v=Oev4Hcd9z_A
IDK the creator. But Thank you!
