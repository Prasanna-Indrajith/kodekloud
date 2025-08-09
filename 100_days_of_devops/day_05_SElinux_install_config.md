# SELinux Install Config

## Question

Following a security audit, the xFusionCorp Industries security team has opted to enhance application and server security with SELinux. To initiate testing, the following requirements have been established for App server 2 in the Stratos Datacenter:

- Install the required SELinux packages.
- Permanently disable SELinux for the time being; it will be re-enabled after necessary configuration changes.
- No need to reboot the server, as a scheduled maintenance reboot is already planned for tonight.
- Disregard the current status of SELinux via the command line; the final status after the reboot should be disabled.

## Answer

SSH into stapp2
```bash
ssh steve@stapp02
```

Get super user privileges
```bash
sudo su -
```

Install all the necessary packages using yum (CentOS)
```bash
yum -y install selinux*
```

Check SELinux status
```bash
sestatus selinux
```

View and edit the config file to disable SELinux
```bash
cat /etc/selinux/config
cat /etc/selinux/config | grep SELINUX
vi /etc/selinux/config
```

Verify changes
```bash
cat /etc/selinux/config | grep SELINUX
sestatus selinux
# > selinux disabled
```

## Additional Details

[Reference](https://www.nbtechsupport.co.in/2021/01/selinux-installation-kodekloud-engineer.html)

## Brief Description About the Environment

**SELinux (Security-Enhanced Linux) Overview**

SELinux is a mandatory access control (MAC) security mechanism originally developed by the National Security Agency (NSA) and integrated into mainstream Linux distributions. It provides an additional layer of security beyond traditional Unix discretionary access control (DAC) permissions.

**Core SELinux Concepts:**

**Access Control Models:**
- **DAC (Discretionary Access Control)**: Traditional Unix permissions (user/group/other)
- **MAC (Mandatory Access Control)**: System-enforced policies that users cannot override
- SELinux implements MAC to prevent privilege escalation and contain security breaches

**SELinux Operating Modes:**
- **Enforcing**: SELinux policy is enforced, violations are blocked and logged
- **Permissive**: SELinux policy violations are logged but not blocked (audit mode)
- **Disabled**: SELinux is completely turned off

**Key Components:**
- **Security Contexts**: Labels assigned to files, processes, and system objects
- **Policies**: Rules that define what actions are allowed between different security contexts
- **Types**: Categories that group similar objects (files, processes, ports)
- **Roles**: Collections of types that define what a user or process can access

**Configuration Files:**
- `/etc/selinux/config`: Main configuration file that sets default policy and mode
- `/etc/selinux/targeted/`: Contains the targeted policy files (most common policy)
- `/var/log/audit/audit.log`: SELinux violation logs and events

**Common SELinux Commands:**
- `sestatus`: Display current SELinux status and configuration
- `getenforce`: Show current enforcement mode
- `setenforce 0|1`: Temporarily set permissive (0) or enforcing (1) mode
- `restorecon`: Restore default security contexts to files
- `setsebool`: Modify SELinux boolean values
- `getsebool`: View SELinux boolean settings

**Why Disable SELinux Initially:**
1. **Application Compatibility**: Some legacy applications may not work properly with SELinux enforced
2. **Policy Customization**: Organizations often need to create custom policies for their specific applications
3. **Testing Phase**: Allows administrators to identify what policies need to be configured
4. **Troubleshooting**: Eliminates SELinux as a variable when diagnosing system issues

**Enterprise Implementation Strategy:**
The approach of installing SELinux packages but initially disabling enforcement is a common enterprise strategy that allows:
- Infrastructure preparation without immediate disruption
- Gradual policy development and testing
- Controlled rollout across multiple servers
- Compliance preparation for security audits

**Security Considerations:**
While SELinux provides significant security benefits, improper configuration can:
- Block legitimate system operations
- Prevent applications from functioning correctly
- Create complex troubleshooting scenarios
- Require specialized knowledge for effective management

The Stratos Datacenter implementation suggests a phased approach where SELinux infrastructure is prepared during scheduled maintenance windows, allowing for proper testing and policy development before full enforcement.
