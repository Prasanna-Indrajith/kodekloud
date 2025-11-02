# Deploy Jenkins CI Server

## Question

The DevOps team at xFusionCorp Industries is initiating the setup of CI/CD pipelines and has decided to utilize Jenkins as their server. Execute the task according to the provided requirements:

1.  Install **Jenkins** on the `jenkins` server using the **`yum` utility only**, and start its service.
      * *Implicit Requirement:* Jenkins requires **Java 17**.
2.  Jenkin's admin user name should be **`theadmin`**, password should be **`Adm!n321`**, full name should be **`Ravi`**, and email should be **`ravi@jenkins.stratos.xfusioncorp.com`**.

-----

## Answer

The following commands must be executed on the **jenkins** server as the `root` user.

### 1\. Install Java 17 (Prerequisite)

Since Jenkins requires Java 17, we must ensure it is installed before attempting to start the service.

```bash
# 1. Install OpenJDK 17 Development Kit using yum
yum install java-17-openjdk-devel -y

# 2. Verify the installation
java -version
```

### 2\. Install Jenkins Repository and Application

```bash
yum install wget -y

# 1. Download and add the Jenkins repository configuration
wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo

# 2. Import the Jenkins GPG key
rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key

# 3. Install Jenkins using yum
yum install jenkins -y
```

### 3\. Start Service and Retrieve Password

Start and enable the service, then get the temporary security password.

```bash
# Reload systemd to pick up configuration changes
systemctl daemon-reload

# Start the Jenkins service
systemctl start jenkins

# Enable Jenkins service to start at boot
systemctl enable jenkins

# Check status to confirm it is active (running)
systemctl status jenkins

# 🔎 Retrieve the initial admin password for web access
cat /var/lib/jenkins/secrets/initialAdminPassword
```

-----

## 🌐 Answer: Web UI Steps

1.  **Access Jenkins:** Navigate to `http://<JENKINS_SERVER_IP>:8080`.
2.  **Unlock Jenkins:** Enter the password retrieved from the file `/var/lib/jenkins/secrets/initialAdminPassword`.
3.  **Install Plugins:** Select **Install suggested plugins**.
4.  **Create First Admin User:** Enter the required credentials:

| Field | Required Value |
| :--- | :--- |
| **Username** | `theadmin` |
| **Password** | `Adm!n321` |
| **Full name** | `Ravi` |
| **E-mail address** | `ravi@jenkins.stratos.xfusioncorp.com` |

5.  Click **Save and Continue** to finalize the setup.

-----

The Jenkins server is now installed with Java 17 support and configured with the required administrator account.