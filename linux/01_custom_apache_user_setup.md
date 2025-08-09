# Custom Apache User Setup

## Question

In response to heightened security concerns, the xFusionCorp Industries security team has opted for custom Apache users for their web applications. Each user is tailored specifically for an application, enhancing security measures. Your task is to create a custom Apache user according to the outlined specifications:

a. Create a user named anita on App server 1 within the Stratos Datacenter.
b. Assign a unique UID 1939 and designate the home directory as /var/www/anita.

## Answer

ssh into App server 01
```bash
ssh tony@172.16.238.10 >> Ir0nM@n
```

Create user with UID 1939
```bash
sudo adduser -u 1939 anita
```

modify the anita's home directory
```bash
sudo usermod -d /var/www/anita -m anita
```

Is it done ?
```bash
grep anita /etc/passwd
# anita:x:1939:1939::/var/www/anita:/bin/bash
```

## Additional Details

Additional commands with usermod

Provide Description
```bash
usermod -c "New apache server admin" anita
```

modify user's home directory
```bash
usermod -d /var/www/anita anita
```

provide shell
```bash
usermod -s /bin/zsh anita
usermod -s /bin/bash anita
```

not give permission to have shell
```bash
usermod -s /sbin/nologin anita
```

login to that user account and do some changes
```bash
su - anita
```

show password - file
```bash
cat /etc/shadow
```

## Brief Description About the Environment

**Apache Web Server User Management & Security Hardening**

Apache HTTP Server typically runs under a dedicated system user account for security isolation. Custom Apache users provide enhanced security by creating application-specific user accounts that limit the scope of potential security breaches and provide fine-grained access control.

**Security Benefits of Custom Apache Users:**

**Principle of Least Privilege:**
- **Application Isolation**: Each application runs under its own user account
- **Limited File Access**: Users can only access files they own or have explicit permissions for
- **Process Separation**: Different applications cannot interfere with each other's processes
- **Damage Containment**: Security breaches are contained within the application's user context

**User ID (UID) Management:**
- **Unique Identification**: Each user has a unique UID for system identification
- **Custom UIDs**: Specific UIDs (like 1939) can be assigned for organizational purposes
- **UID Ranges**: System users typically use UIDs below 1000, regular users above 1000
- **Consistency**: Custom UIDs ensure consistent user identification across systems

**Apache User Configuration:**

**Traditional Apache Setup:**
- **Default Users**: Common users like `apache`, `www-data`, or `httpd`
- **Shared Context**: All web applications run under the same user
- **Security Risk**: Compromise of one application affects all applications
- **File Permissions**: All web files must be readable by the single Apache user

**Custom Apache User Benefits:**
- **Dedicated Users**: Each application gets its own system user
- **Isolated Permissions**: Applications can only access their own files
- **Enhanced Monitoring**: Process and file access can be tracked per application
- **Compliance**: Meets security requirements for application separation

**Home Directory Considerations:**

**Web Application Directory Structure:**
- **Standard Location**: `/var/www/` is the conventional web root directory
- **Application-Specific**: `/var/www/anita` provides dedicated space for the application
- **File Ownership**: All application files owned by the custom user (anita)
- **Permission Structure**: Proper ownership enables secure file operations

**Directory Permissions:**
```
/var/www/anita/
├── public_html/          # Web-accessible files
├── logs/                 # Application-specific logs
├── config/               # Configuration files
└── temp/                 # Temporary files
```

**User Management Commands Explained:**

**User Creation:**
- `adduser -u 1939 anita`: Creates user with specific UID
- `useradd` vs `adduser`: Different tools with varying default behaviors
- `-u` flag: Specifies custom UID instead of auto-assigned
- User creation updates `/etc/passwd`, `/etc/shadow`, and `/etc/group`

**User Modification:**
- `usermod -d /var/www/anita -m anita`: Changes home directory and moves files
- `-d` flag: Specifies new home directory
- `-m` flag: Moves contents from old home directory to new location
- `-c` flag: Adds or modifies user comment/description
- `-s` flag: Changes user's default shell

**Shell Configuration Options:**
- `/bin/bash`: Interactive shell for administrative access
- `/bin/zsh`: Alternative interactive shell with enhanced features
- `/sbin/nologin`: Prevents interactive login while allowing service execution
- Shell choice depends on whether human access is needed

**Apache Integration Workflow:**

**Web Server Configuration:**
1. **Virtual Host Setup**: Configure Apache virtual host for the application
2. **User Assignment**: Set Apache to run application processes as custom user
3. **File Permissions**: Ensure proper ownership and permissions for web files
4. **Process Management**: Configure process management tools to use custom user

**Example Apache Virtual Host Configuration:**
```apache
<VirtualHost *:80>
    ServerName anita.example.com
    DocumentRoot /var/www/anita/public_html
    User anita
    Group anita
    
    <Directory /var/www/anita/public_html>
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>
```

**Security Hardening Practices:**

**Access Control:**
- **File Permissions**: Restrict file access to application user only
- **Directory Permissions**: Prevent unauthorized directory traversal
- **Log Separation**: Maintain separate log files per application
- **Process Isolation**: Each application runs in its own process context

**Monitoring and Auditing:**
- **User Activity**: Track file access and process execution per user
- **Resource Usage**: Monitor CPU, memory, and disk usage per application
- **Security Events**: Log authentication and authorization events
- **Compliance Reporting**: Generate reports for security audits

**Enterprise Implementation:**

**Stratos Datacenter Context:**
- **Multi-Application Environment**: Multiple web applications requiring isolation
- **Security Compliance**: Meeting enterprise security standards
- **Scalability**: Consistent user management across multiple app servers
- **Maintenance**: Simplified application deployment and management

**Best Practices:**
- **Naming Conventions**: Use descriptive usernames that indicate application purpose
- **Documentation**: Maintain records of custom users and their purposes
- **Automation**: Script user creation for consistent deployment
- **Backup**: Include user account information in system backups
- **Regular Audits**: Periodically review custom user accounts and permissions

**Troubleshooting Common Issues:**
- **Permission Denied**: Check file ownership and directory permissions
- **UID Conflicts**: Ensure UID uniqueness across the system
- **Home Directory**: Verify proper home directory creation and permissions
- **Apache Configuration**: Confirm virtual host configuration matches user setup
- **Process Ownership**: Verify Apache processes run under correct user context

This approach enables xFusionCorp Industries to implement robust security measures while maintaining proper separation of web applications within their infrastructure.
