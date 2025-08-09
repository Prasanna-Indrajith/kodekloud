# Setup Git Repository

## Question

The Nautilus development team has provided requirements to the DevOps team for a new application development project, specifically requesting the establishment of a Git repository. Follow the instructions below to create the Git repository on the Storage server in the Stratos DC:

Utilize yum to install the git package on the Storage Server. Create a bare repository named /opt/news.git (ensure exact name usage).

## Answer

ssh into "storage server"
```bash
ssh natasha@172.16.238.15
-> Bl@kW
```

install git
```bash
sudo yum install git
```

create "bare" repository
```bash
sudo git init --bare /opt/news.git
```

exit the server
```bash
exit
```

## Additional Details

## Brief Description About the Environment

**Git Version Control & Bare Repository Management**

Git is a distributed version control system that tracks changes in source code and enables collaborative software development. In enterprise environments, Git repositories are often centralized on dedicated storage servers to provide a single source of truth for development teams.

**Core Git Concepts:**

**Repository Types:**
- **Working Repository**: Contains a working directory with checked-out files and a `.git` directory
- **Bare Repository**: Contains only Git data (no working directory), designed for sharing and central storage
- Bare repositories are ideal for server-side storage as they prevent conflicts when multiple developers push changes

**Bare Repository Characteristics:**
- **No working directory**: Cannot check out files directly
- **Optimized for sharing**: Multiple developers can push/pull without conflicts
- **Server-side storage**: Typically used on central servers for team collaboration
- **Naming convention**: Often ends with `.git` extension to indicate it's a bare repository

**Git Installation & Setup:**

**Package Installation:**
- **RHEL/CentOS**: `yum install git` or `dnf install git`
- **Ubuntu/Debian**: `apt install git`
- **Verification**: `git --version` to confirm installation

**Repository Initialization:**
- `git init`: Creates a regular repository with working directory
- `git init --bare`: Creates a bare repository for server-side storage
- `git clone --bare`: Creates a bare clone of existing repository

**Enterprise Git Workflow:**

**Centralized Model:**
1. **Central Repository**: Bare repository on storage server (like `/opt/news.git`)
2. **Developer Clones**: Each developer clones the central repository locally
3. **Push/Pull Operations**: Developers push commits to and pull updates from central repository
4. **Backup Strategy**: Central repository serves as authoritative source

**Storage Server Role:**
- **Centralized Storage**: Houses all Git repositories for the organization
- **Access Control**: Can implement user permissions and access restrictions
- **Backup Target**: Single point for repository backup and disaster recovery
- **Network Access**: Accessible by development team via SSH, HTTP, or Git protocol

**Common Git Operations with Bare Repositories:**

**Developer Workflow:**
```bash
# Clone from bare repository
git clone natasha@172.16.238.15:/opt/news.git

# Make changes and commit locally
git add .
git commit -m "Your changes"

# Push to central bare repository
git push origin main

# Pull updates from central repository
git pull origin main
```

**Repository Management:**
- **Permissions**: Ensure proper file ownership and permissions on the storage server
- **Hooks**: Can implement server-side hooks for automated testing, deployment, or notifications
- **Maintenance**: Periodic garbage collection and repository optimization
- **Monitoring**: Track repository size, access patterns, and performance

**Security Considerations:**
- **SSH Access**: Secure access to storage server using SSH keys
- **User Permissions**: Control who can read/write to repositories
- **Network Security**: Firewall rules for Git traffic
- **Audit Logging**: Track repository access and changes

**Stratos DC Architecture:**
The setup suggests a typical enterprise architecture where:
- **Storage Server (172.16.238.15)**: Dedicated server for Git repositories and shared storage
- **Development Team**: Distributed developers accessing central repositories
- **DevOps Integration**: Git repositories integrated with CI/CD pipelines
- **Project Organization**: Structured repository naming (news.git for news application project)

**Best Practices:**
- **Naming Conventions**: Use descriptive names with `.git` suffix for bare repositories
- **Directory Structure**: Organize repositories in logical directory structures (e.g., `/opt/git/project-name.git`)
- **Backup Strategy**: Regular backups of Git repositories to prevent data loss
- **Access Management**: Implement proper user access controls and authentication
- **Documentation**: Maintain documentation of repository purposes and access procedures

This foundation enables the Nautilus development team to collaborate effectively on the news application project while maintaining proper version control and code management practices.
