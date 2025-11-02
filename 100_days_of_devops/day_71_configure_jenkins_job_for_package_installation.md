# Configure Jenkins Job for Package Installation

## Question

Some new requirements have come up to install and configure some packages on the Nautilus infrastructure under Stratos Datacenter. The Nautilus DevOps team installed and configured a new Jenkins server so they wanted to create a Jenkins job to automate this task. Find below more details and complete the task accordingly:

1. Access the Jenkins UI by clicking on the Jenkins button in the top bar. Log in using the credentials: username admin and password Adm!n321.

2. Create a new Jenkins job named install-packages and configure it with the following specifications:

Add a string parameter named PACKAGE.
Configure the job to install a package specified in the $PACKAGE parameter on the storage server within the Stratos Datacenter.

## Answer

Configure Jenkins Job for Package Installation

To configure a Jenkins job for package installation on KodeKloud, follow these steps:

1.  **Install Required Plugins**: Access the Jenkins UI using the credentials `admin` and `Adm!n321`. Navigate to `Manage Jenkins > Manage Plugins`, go to the `Available` tab, search for and install the `SSH Plugin`. After installation, select the option to restart Jenkins when complete and refresh the browser page to ensure the UI is functional 

2.  **Create SSH Credentials**: Go to `Manage Jenkins > Manage Credentials`. Under `Global` (scoped to Jenkins), click `Add Credentials`. Set the username to `natasha`, the password to `Bl@kW`, and assign an ID of `storage` 

3.  **Configure SSH Host**: Navigate to `Manage Jenkins > Configure System`. Under `SSH Remote Hosts`, click `Add Host`. Enter the hostname `ststor01.stratos.xfusioncorp.com`, port `22`, and select the credentials `storage` from the list. Click `Check Connection` to verify the setup is successful 

4.  **Create the Jenkins Job**: Click `New Item`, name the job `install-packages`, and select `Freestyle project`. Click `OK` 

5.  **Add a Parameter**: Under `This project is parameterized`, add a `String Parameter`. Set the name to `PACKAGE` and provide a default value (e.g., the name of a package to install) 

6.  **Add Build Step**: Under `Build`, add a `Build Step` of type `Execute shell script on remote host using SSH`. Select the SSH site `natasha@ststor01.stratos.xfusioncorp.com:22`  In the command text area, enter the following script: `echo 'Bl@kW' | sudo -S yum install -y $PACKAGE`  This command uses the `echo` command to pipe the password to `sudo` via the `-S` flag, which reads the password from standard input, allowing the `yum install` command to run non-interactively 

7.  **Save and Run**: Save the job configuration. Run the build, providing a package name as the `PACKAGE` parameter value. The job should successfully install the specified package on the remote storage server 