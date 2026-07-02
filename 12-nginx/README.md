
# 🚀 Phase 12 (Part 1) — Installing & Configuring Jenkins on AWS EC2 using Docker

## 🎯 Objective

In this phase, we will install Jenkins as a Docker container on the same AWS EC2 instance where Magento is deployed. Jenkins will later be used to automate the build, testing, and deployment of the Magento application.

By the end of this phase, you will have:

- Dockerized Jenkins
- Persistent Jenkins data
- Jenkins accessible via browser
- Initial administrator setup completed
- Required plugins installed
- Docker integration configured
- Ready for CI/CD pipeline creation

---

# Architecture

```

                    GitHub Repository
                           │
                           │
                     (Webhook Later)
                           │
                    Jenkins Container
                           │
             ┌─────────────┴──────────────┐
             │                            │
        Docker CLI                  Docker Engine
             │                            │
             └──────────────┬─────────────┘
                            │
                     Magento Containers

```

---

# Prerequisites

Before starting this phase ensure:

- AWS EC2 instance is running
- Docker is installed
- Docker Compose is installed
- Magento stack is working
- Domain and SSL configured
- SSH access available

Verify Docker

```bash
docker --version
```

Verify Docker Compose

```bash
docker compose version
```

---

# Why Jenkins?

Jenkins is one of the most popular open-source automation servers used for:

- Continuous Integration (CI)
- Continuous Deployment (CD)
- Automated Builds
- Automated Testing
- Infrastructure Automation
- Docker Deployment
- Kubernetes Deployment
- Cloud Deployments

---

# Why Run Jenkins in Docker?

Benefits:

- Easy installation
- Portable
- Easy upgrades
- Isolated environment
- Persistent data using Docker volumes
- No dependency conflicts

---

# Directory Structure

Create a directory for Jenkins.

```bash
mkdir ~/jenkins
```

Move into it.

```bash
cd ~/jenkins
```

---

# Create Docker Volume

Create a persistent volume for Jenkins.

```bash
docker volume create jenkins_home
```

Verify

```bash
docker volume ls
```

Expected

```
jenkins_home
```

---

# Create Docker Network (Optional)

If Jenkins needs to communicate with Magento containers later, it should use the same Docker network.

Check existing networks

```bash
docker network ls
```

Example

```
magento-net
```

---

# Pull Jenkins Image

Download the latest LTS image.

```bash
docker pull jenkins/jenkins:lts
```

Verify

```bash
docker images
```

---

# Create docker-compose.yml

Create the file.

```bash
touch docker-compose.yml
```

Example configuration

```yaml
services:

  jenkins:

    image: jenkins/jenkins:lts

    container_name: jenkins

    restart: unless-stopped

    privileged: true

    user: root

    ports:

      - "8080:8080"

      - "50000:50000"

    volumes:

      - jenkins_home:/var/jenkins_home

      - /var/run/docker.sock:/var/run/docker.sock

      - /usr/bin/docker:/usr/bin/docker

    networks:

      - magento-net

volumes:

  jenkins_home:

networks:

  magento-net:

    external: true
```

---

# Explanation

### privileged: true

Allows Jenkins to access Docker.

---

### user: root

Allows plugin installation and Docker execution.

---

### docker.sock

Allows Jenkins to communicate with Docker Engine.

---

### docker binary

Allows Jenkins to execute Docker commands.

---

### Persistent Volume

Stores:

- Jobs
- Plugins
- Credentials
- Pipelines
- Workspace

Even if the container is removed, Jenkins data remains.

---

# Start Jenkins

```bash
docker compose up -d
```

Verify

```bash
docker ps
```

Expected

```
jenkins
```

---

# Check Logs

```bash
docker logs -f jenkins
```

Wait until you see

```
Jenkins is fully up and running
```

---

# Access Jenkins

Browser

```
http://<EC2-PUBLIC-IP>:8080
```

Example

```
http://34.xxx.xxx.xxx:8080
```

---

# Open Port 8080

AWS Console

```
EC2

↓

Security Groups

↓

Inbound Rules

↓

Add Rule

Port 8080

Source:

Your IP

or

Anywhere (Testing only)
```

---

# Retrieve Initial Admin Password

Run

```bash
docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```

Example

```
c4d2fa31b28d987...
```

Copy it.

Paste into Jenkins.

Click

```
Continue
```

---

# Install Suggested Plugins

Choose

```
Install Suggested Plugins
```

Wait until installation completes.

---

# Create First Admin User

Provide

```
Username

Password

Email

Full Name
```

Click

```
Save and Continue
```

---

# Configure Jenkins URL

Example

```
http://34.xxx.xxx.xxx:8080/
```

Later

```
https://jenkins.yourdomain.com
```

---

# Jenkins Dashboard

You should now see

```
Welcome to Jenkins
```

Dashboard

```
New Item

Build History

Manage Jenkins

Credentials

Pipeline

Nodes

Clouds

```

---

# Verify Jenkins Container

```bash
docker ps
```

Expected

```
jenkins
```

---

# Verify Volume

```bash
docker volume ls
```

Expected

```
jenkins_home
```

---

# Verify Docker Access

Run

```bash
docker exec -it jenkins docker version
```

Expected

```
Client

Server
```

This confirms Jenkins can communicate with Docker.

---

# Restart Jenkins

```bash
docker restart jenkins
```

Verify

```
Dashboard opens successfully
```

---

# Useful Commands

View logs

```bash
docker logs jenkins
```

Follow logs

```bash
docker logs -f jenkins
```

Restart

```bash
docker restart jenkins
```

Stop

```bash
docker stop jenkins
```

Start

```bash
docker start jenkins
```

Remove

```bash
docker rm -f jenkins
```

---

# Troubleshooting

## Jenkins not opening

Verify

```bash
docker ps
```

---

## Port already in use

Check

```bash
sudo lsof -i:8080
```

Kill process if needed.

---

## Permission denied

Verify

```bash
ls -l /var/run/docker.sock
```

---

## Cannot communicate with Docker

Verify

```bash
docker exec jenkins docker version
```

---

## Container restarting

Check

```bash
docker logs jenkins
```

---

# Best Practices

✔ Use Docker volumes

✔ Keep Jenkins LTS version

✔ Backup Jenkins volume regularly

✔ Restrict port 8080 access

✔ Never expose Jenkins publicly without authentication

✔ Use HTTPS in production

✔ Keep plugins updated

---

# Validation Checklist

| Validation | Status |
|------------|--------|
| Docker Installed | ✅ |
| Jenkins Image Pulled | ✅ |
| Jenkins Volume Created | ✅ |
| Docker Compose Created | ✅ |
| Jenkins Container Running | ✅ |
| Jenkins Dashboard Accessible | ✅ |
| Initial Password Retrieved | ✅ |
| Admin User Created | ✅ |
| Suggested Plugins Installed | ✅ |
| Docker Accessible from Jenkins | ✅ |

---

# Screenshots

Capture the following screenshots.

---

## Jenkins Docker Image

```
screenshots/jenkins-image.png
```

```markdown
![Jenkins Image](../screenshots/jenkins-image.png)
```

---

## Docker Compose File

```
screenshots/jenkins-compose.png
```

```markdown
![Docker Compose](../screenshots/jenkins-compose.png)
```

---

## Running Jenkins Container

```
screenshots/jenkins-container.png
```

```markdown
![Running Container](../screenshots/jenkins-container.png)
```

---

## Jenkins Initial Password

```
screenshots/jenkins-password.png
```

```markdown
![Initial Password](../screenshots/jenkins-password.png)
```

---

## Plugin Installation

```
screenshots/plugin-installation.png
```

```markdown
![Plugins](../screenshots/plugin-installation.png)
```

---

## Jenkins Dashboard

```
screenshots/jenkins-dashboard.png
```

```markdown
![Dashboard](../screenshots/jenkins-dashboard.png)
```

---

## Docker Version inside Jenkins

```
screenshots/docker-version-jenkins.png
```

```markdown
![Docker Integration](../screenshots/docker-version-jenkins.png)
```

---

# Skills Demonstrated

By completing this phase, you have demonstrated the ability to:

- Deploy Jenkins using Docker
- Configure persistent Jenkins storage with Docker volumes
- Connect Jenkins to the Docker Engine
- Securely initialize Jenkins and create an administrator account
- Install essential Jenkins plugins
- Prepare a production-ready Jenkins environment for CI/CD automation

---

# 🚀 Next Phase

**Phase 12 (Part 2) — GitHub Integration & Jenkins Configuration**

In the next part, we will:

- Configure Jenkins Global Tools (Git, JDK, Maven)
- Install additional required plugins
- Create GitHub Personal Access Token (PAT)
- Add GitHub credentials to Jenkins
- Configure SSH keys for Jenkins
- Connect Jenkins with the GitHub repository
- Configure GitHub Webhooks for automatic builds
- Validate GitHub-to-Jenkins connectivity



# 🚀 Phase 12 (Part 2A) — Configuring Jenkins, Installing Plugins & Connecting to GitHub

## 🎯 Objective

In this phase, we will configure Jenkins after installation by:

- Installing required plugins
- Configuring Global Tools
- Installing Git
- Installing Docker CLI
- Installing Maven
- Installing JDK
- Creating GitHub Personal Access Token (PAT)
- Adding GitHub credentials into Jenkins
- Verifying GitHub authentication

At the end of this phase Jenkins will be fully prepared to build Magento projects directly from GitHub.

---

# Architecture

```

Developer
│
│ Push Code
▼

GitHub Repository
│
│ Authenticate using PAT
▼

Jenkins
│
├── Git
├── Docker
├── Maven
└── JDK

```

---

# Prerequisites

Before starting this phase ensure:

- Jenkins is installed
- Jenkins dashboard is accessible
- Docker is running
- GitHub repository exists
- GitHub account is active

---

# Login to Jenkins

Open

```
http://<EC2-PUBLIC-IP>:8080
```

Login using

```
Username
Password
```

---

# Update Jenkins

Navigate to

```
Manage Jenkins

↓

Plugins
```

Click

```
Updates
```

Install all available updates.

Restart Jenkins if required.

---

# Install Required Plugins

Navigate to

```
Manage Jenkins

↓

Plugins

↓

Available Plugins
```

Search and install the following plugins.

---

## Git Plugin

Provides Git integration.

Search

```
Git
```

Install

```
Git Plugin
```

---

## Pipeline Plugin

Search

```
Pipeline
```

Install

```
Pipeline
```

---

## Docker Plugin

Search

```
Docker
```

Install

```
Docker
```

---

## Docker Pipeline Plugin

Search

```
Docker Pipeline
```

Install

```
Docker Pipeline
```

---

## SSH Agent Plugin

Search

```
SSH Agent
```

Install

```
SSH Agent
```

---

## Credentials Plugin

Usually installed by default.

Verify it exists.

---

## GitHub Plugin

Search

```
GitHub
```

Install

```
GitHub Plugin
```

---

## GitHub Integration Plugin

Install

```
GitHub Integration
```

---

## Blue Ocean (Optional)

Provides modern UI.

Search

```
Blue Ocean
```

Install if desired.

---

# Restart Jenkins

Navigate

```
Manage Jenkins

↓

Restart Jenkins
```

or

```
docker restart jenkins
```

---

# Configure Global Tools

Navigate

```
Manage Jenkins

↓

Tools
```

---

# Configure Git

Click

```
Git Installations
```

Add

```
Name

Git
```

Check

```
Install automatically
```

Choose latest version.

Save.

---

# Configure JDK

Navigate

```
JDK Installations
```

Name

```
JDK17
```

Check

```
Install automatically
```

Choose

```
Temurin 17
```

Save.

---

# Configure Maven

Navigate

```
Maven Installations
```

Name

```
Maven3
```

Check

```
Install automatically
```

Latest version.

Save.

---

# Verify Tool Configuration

You should now have

```
Git

JDK17

Maven3
```

configured inside Jenkins.

---

# Install Docker CLI Inside Jenkins Container

Enter container

```bash
docker exec -it jenkins bash
```

Update packages

```bash
apt update
```

Install Docker CLI

```bash
apt install docker.io -y
```

Verify

```bash
docker version
```

Exit

```bash
exit
```

---

# Verify Docker Access

Run

```bash
docker exec jenkins docker ps
```

You should see all running containers.

---

# Create GitHub Personal Access Token

Login to GitHub.

Navigate

```
Settings

↓

Developer Settings

↓

Personal Access Tokens

↓

Fine-grained Tokens
```

Click

```
Generate New Token
```

---

# Token Configuration

Repository Access

```
Only selected repositories
```

Select

```
magento2-aws-docker-devops
```

Permissions

```
Contents

Read & Write
```

Metadata

```
Read Only
```

Expiration

```
90 Days

or

No Expiration
```

Generate token.

Copy immediately.

Example

```
github_pat_xxxxxxxxxxxxxxxxx
```

---

# Store Token Securely

Never commit PAT to GitHub.

Never store it inside Jenkinsfile.

Always use Jenkins Credentials.

---

# Add GitHub Credentials

Navigate

```
Manage Jenkins

↓

Credentials

↓

System

↓

Global Credentials

↓

Add Credentials
```

Kind

```
Username with Password
```

Username

```
GitHub Username
```

Password

```
GitHub PAT
```

ID

```
github-pat
```

Description

```
GitHub Personal Access Token
```

Click

```
Save
```

---

# Verify Credentials

You should now see

```
github-pat
```

inside

```
Global Credentials
```

---

# Test GitHub Authentication

Create a temporary freestyle job.

Source Code Management

```
Git
```

Repository URL

```
https://github.com/<username>/magento2-aws-docker-devops.git
```

Credentials

```
github-pat
```

Click

```
Test Connection
```

Expected

```
Connection Successful
```

---

# Verify Jenkins Can Clone Repository

Run build.

Console Output

Expected

```
Fetching upstream changes

Cloning repository

Checking out Revision

Finished SUCCESS
```

---

# Best Practices

✔ Use Jenkins Credentials

✔ Never hardcode PAT

✔ Keep plugins updated

✔ Remove unused plugins

✔ Use latest LTS Jenkins

✔ Use least privilege PAT

✔ Rotate PAT periodically

---

# Validation Checklist

| Validation | Status |
|------------|--------|
| Jenkins Running | ✅ |
| Plugins Installed | ✅ |
| Git Configured | ✅ |
| Maven Configured | ✅ |
| JDK Configured | ✅ |
| Docker CLI Installed | ✅ |
| Docker Accessible | ✅ |
| GitHub PAT Created | ✅ |
| Jenkins Credentials Added | ✅ |
| Repository Authentication Successful | ✅ |

---

# Useful Commands

Check Jenkins container

```bash
docker ps
```

Restart Jenkins

```bash
docker restart jenkins
```

Open Jenkins shell

```bash
docker exec -it jenkins bash
```

Check Docker

```bash
docker version
```

List plugins (inside Jenkins)

```bash
ls /var/jenkins_home/plugins
```

---

# Troubleshooting

## Git Not Found

Reconfigure Git Tool.

Restart Jenkins.

---

## Maven Missing

Check

```
Manage Jenkins

↓

Tools
```

---

## Docker Permission Denied

Verify

```bash
docker exec jenkins docker ps
```

If permission denied, verify

```
/var/run/docker.sock
```

is mounted correctly.

---

## Authentication Failed

Verify

- PAT not expired
- Correct repository
- Correct username
- Correct permissions

---

## Repository Not Found

Check repository URL

```
https://github.com/<username>/<repo>.git
```

---

# Screenshots

Capture the following screenshots.

---

## Installed Plugins

```
screenshots/plugins-installed.png
```

```markdown
![Plugins](../screenshots/plugins-installed.png)
```

---

## Global Tool Configuration

```
screenshots/global-tools.png
```

```markdown
![Tools](../screenshots/global-tools.png)
```

---

## Git Configuration

```
screenshots/git-config.png
```

```markdown
![Git](../screenshots/git-config.png)
```

---

## Maven Configuration

```
screenshots/maven-config.png
```

```markdown
![Maven](../screenshots/maven-config.png)
```

---

## JDK Configuration

```
screenshots/jdk-config.png
```

```markdown
![JDK](../screenshots/jdk-config.png)
```

---

## GitHub PAT

(Blur the token)

```
screenshots/github-pat.png
```

```markdown
![PAT](../screenshots/github-pat.png)
```

---

## Jenkins Credentials

```
screenshots/github-credentials.png
```

```markdown
![Credentials](../screenshots/github-credentials.png)
```

---

## Repository Connection Successful

```
screenshots/github-test-connection.png
```

```markdown
![Repository Connection](../screenshots/github-test-connection.png)
```

---

# Skills Demonstrated

By completing this phase, you have demonstrated the ability to:

- Configure Jenkins Global Tools
- Install and manage Jenkins plugins
- Integrate Docker with Jenkins
- Securely authenticate Jenkins with GitHub
- Use GitHub Personal Access Tokens (PAT)
- Manage Jenkins credentials securely
- Prepare Jenkins for automated CI/CD workflows

---

# 🚀 Next Phase

**Phase 12 (Part 2B) — Configuring SSH Keys, GitHub Webhooks & Automatic Build Triggers**

In the next part, we will:

- Generate SSH keys for Jenkins
- Configure GitHub SSH authentication
- Add SSH credentials to Jenkins
- Configure GitHub Webhooks
- Trigger automatic builds on code push
- Validate webhook delivery
- Troubleshoot common webhook issues
- Prepare Jenkins for pipeline automation

# Phase 12 - CI/CD Pipeline (Part 2B-1)

# Jenkins SSH Authentication with GitHub

---

# Objective

In this phase, Jenkins is configured to communicate securely with GitHub using SSH authentication instead of HTTPS.

Using SSH eliminates the need for GitHub Personal Access Tokens (PAT) and provides a more secure and production-ready Git workflow.

At the end of this phase Jenkins will be able to:

- Clone private repositories
- Push code (if required)
- Authenticate securely using SSH Keys
- Work with GitHub Webhooks

---

# Architecture

```
                GitHub Repository
                       ▲
                       │
               SSH Authentication
                       │
               Jenkins EC2 Server
                       │
               SSH Key Pair
```

---

# Why SSH instead of HTTPS?

HTTPS requires:

- Username
- Password / Personal Access Token

SSH requires only:

- Private Key
- Public Key

Benefits:

- More secure
- Easier automation
- Recommended by GitHub
- No PAT expiration issues

---

# Step 1 — Login to Jenkins Server

SSH into Jenkins EC2 instance.

```bash
ssh -i assessment.pem ubuntu@<JENKINS_PUBLIC_IP>
```

Example

```bash
ssh -i assessment.pem ubuntu@34.xx.xx.xx
```

---

# Step 2 — Verify Git Installation

```bash
git --version
```

Expected Output

```text
git version 2.x.x
```

---

# Step 3 — Check Existing SSH Keys

```bash
ls -la ~/.ssh
```

Example

```text
authorized_keys
known_hosts
```

If **id_rsa** does not exist, generate a new SSH key.

---

# Step 4 — Generate SSH Key

Run

```bash
ssh-keygen -t rsa -b 4096 -C "jenkins@magento"
```

Press Enter for every prompt.

```
Enter file in which to save the key:
/home/ubuntu/.ssh/id_rsa
```

Press Enter.

```
Enter passphrase:
```

Leave blank.

```
Confirm passphrase:
```

Leave blank.

---

# Generated Files

```
~/.ssh/id_rsa
```

Private Key

```
~/.ssh/id_rsa.pub
```

Public Key

---

# Step 5 — Verify Generated Keys

```bash
ls ~/.ssh
```

Expected

```
id_rsa
id_rsa.pub
known_hosts
authorized_keys
```

---

# Step 6 — Display Public Key

```bash
cat ~/.ssh/id_rsa.pub
```

Example

```text
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQC....
```

Copy the complete key.

---

# Step 7 — Add Public Key to GitHub

Open

GitHub

↓

Settings

↓

SSH and GPG Keys

↓

New SSH Key

---

Fill

Title

```
Jenkins Server
```

Key

Paste

```
ssh-rsa AAAAB3...
```

Click

```
Add SSH Key
```

---

# Step 8 — Test SSH Authentication

Run

```bash
ssh -T git@github.com
```

First time

```
Are you sure you want to continue connecting?
```

Type

```
yes
```

Expected Output

```text
Hi username!

You've successfully authenticated.
```

---

# Step 9 — Verify Repository Access

Clone using SSH.

Example

```bash
git clone git@github.com:sutar-rushikesh/magento2-aws-docker-devops.git
```

Expected

```text
Cloning into 'magento2-aws-docker-devops'...
Receiving objects...
```

No username or password should be requested.

---

# Step 10 — Copy Private Key

Display private key.

```bash
cat ~/.ssh/id_rsa
```

Copy everything including

```
-----BEGIN OPENSSH PRIVATE KEY-----
```

and

```
-----END OPENSSH PRIVATE KEY-----
```

---

# Step 11 — Open Jenkins Dashboard

Navigate to

```
Manage Jenkins
```

↓

```
Credentials
```

↓

```
System
```

↓

```
Global Credentials
```

↓

```
Add Credentials
```

---

# Step 12 — Configure SSH Credential

Kind

```
SSH Username with private key
```

Username

```
git
```

Private Key

```
Enter directly
```

Paste

```
id_rsa
```

ID

```
github-ssh
```

Description

```
GitHub SSH Authentication
```

Save.

---

# Step 13 — Verify Credential

Navigate

```
Manage Jenkins

↓

Credentials
```

You should see

```
github-ssh
```

---

# Step 14 — Convert Repository URL

Old HTTPS

```text
https://github.com/sutar-rushikesh/magento2-aws-docker-devops.git
```

New SSH

```text
git@github.com:sutar-rushikesh/magento2-aws-docker-devops.git
```

Always use the SSH URL inside Jenkins jobs.

---

# Step 15 — Configure Jenkins Job

Open

```
Magento Pipeline
```

↓

Configure

↓

Source Code Management

↓

Git

Repository URL

```text
git@github.com:sutar-rushikesh/magento2-aws-docker-devops.git
```

Credentials

```
github-ssh
```

Save.

---

# Step 16 — Test Git Connectivity

Click

```
Build Now
```

Console Output should show

```text
Fetching changes from Git repository
```

```text
using credential github-ssh
```

```text
Checking out Revision...
```

No authentication errors should appear.

---

# Validation Checklist

- Jenkins can connect to GitHub via SSH
- SSH key pair generated successfully
- Public key added to GitHub
- Private key stored in Jenkins Credentials
- Repository URL changed to SSH
- Jenkins job clones repository successfully
- No username/password prompts
- No PAT required

---

# Common Errors

## Permission denied (publickey)

Cause

Public key not added to GitHub.

Solution

Verify

```bash
cat ~/.ssh/id_rsa.pub
```

Paste it again in GitHub SSH Keys.

---

## Repository not found

Cause

Wrong SSH repository URL.

Correct format

```text
git@github.com:username/repository.git
```

---

## Host key verification failed

Run

```bash
ssh -T git@github.com
```

Accept

```
yes
```

This updates the `known_hosts` file.

---

# Screenshots to Capture

- SSH key generation
- GitHub SSH Keys page
- Jenkins Credentials page
- Jenkins Git SCM configuration
- Successful SSH authentication (`ssh -T git@github.com`)
- Successful Jenkins build using SSH

---

# Skills Demonstrated

- Git
- GitHub
- SSH Authentication
- Jenkins Credentials Management
- Secure Repository Access
- DevOps Security Best Practices
- CI/CD Pipeline Configuration

---

# Next Phase

**Phase 12 – Part 2B-2**

Topics covered:

- GitHub Webhooks
- Automatic Jenkins Builds
- Webhook Troubleshooting
- Build Verification
- End-to-End CI Trigger Testing

# Phase 12 - CI/CD Pipeline (Part 2B-2)

# Configuring GitHub Webhooks for Automatic Jenkins Builds

---

# Objective

In the previous phase, Jenkins was configured to authenticate with GitHub using SSH.

In this phase, GitHub Webhooks are configured so that every push to the GitHub repository automatically triggers a Jenkins build.

This eliminates the need to manually click **Build Now** after every code change.

---

# Architecture

```

Developer
│
│ git push
▼
GitHub Repository
│
│ HTTP POST (Webhook)
▼
Jenkins Server
│
▼
Pipeline Starts Automatically

```

---

# Why Use GitHub Webhooks?

Without Webhooks

```

Developer Pushes Code

↓

Login Jenkins

↓

Click Build Now

↓

Pipeline Executes

```

With Webhooks

```

Developer Pushes Code

↓

GitHub Sends Webhook

↓

Jenkins Receives Event

↓

Pipeline Starts Automatically

```

Advantages

- Fully automated
- Faster deployments
- Industry standard CI/CD workflow
- No manual intervention

---

# Prerequisites

Before starting this phase, ensure:

- Jenkins is installed
- GitHub repository is connected using SSH
- Jenkins Pipeline Job exists
- GitHub repository is accessible
- Public domain or Elastic IP is configured
- Jenkins is reachable from the internet

Example

```
http://<JENKINS_PUBLIC_IP>:8080
```

or

```
https://jenkins.example.com
```

---

# Step 1 – Install GitHub Plugin

Open Jenkins

```
Manage Jenkins
```

↓

```
Plugins
```

↓

```
Available Plugins
```

Search

```
GitHub
```

Install

```
GitHub Plugin
```

Also ensure the following plugins are installed:

- Git Plugin
- GitHub Branch Source Plugin
- Pipeline Plugin

Restart Jenkins if required.

---

# Step 2 – Configure Jenkins Job

Open

```
Dashboard

↓

Magento Pipeline
```

Click

```
Configure
```

Scroll to

```
Build Triggers
```

Enable

```
GitHub hook trigger for GITScm polling
```

Save.

---

# Step 3 – Obtain Webhook URL

Webhook URL format

```
http://JENKINS_IP:8080/github-webhook/
```

Example

```
http://34.xx.xx.xx:8080/github-webhook/
```

If HTTPS is configured

```
https://jenkins.example.com/github-webhook/
```

Notice the trailing slash (`/`) is mandatory.

---

# Step 4 – Open GitHub Repository

Navigate to your repository.

Example

```
magento2-aws-docker-devops
```

Click

```
Settings
```

↓

```
Webhooks
```

↓

```
Add Webhook
```

---

# Step 5 – Configure Webhook

Payload URL

```
http://<JENKINS_IP>:8080/github-webhook/
```

Example

```
http://34.xx.xx.xx:8080/github-webhook/
```

Content Type

```
application/json
```

Secret

Leave blank (optional)

SSL Verification

```
Enable SSL Verification
```

or

```
Disable
```

if testing without HTTPS.

Events

Choose

```
Just the push event
```

Active

```
✓ Enabled
```

Click

```
Add Webhook
```

---

# Step 6 – Test Webhook

Immediately after creation GitHub sends a test payload.

Open

```
GitHub

↓

Settings

↓

Webhooks
```

Click the webhook.

You should see

```
Recent Deliveries
```

Expected

```
Response Code

200
```

This confirms Jenkins received the payload.

---

# Step 7 – Verify Jenkins Logs

Open Jenkins.

Click

```
Magento Pipeline

↓

Build History
```

A new build should appear automatically.

Open

```
Console Output
```

Expected

```
Started by GitHub push
```

Example

```
Started by GitHub push by Rushikesh
```

This confirms the webhook is functioning.

---

# Step 8 – Verify Automatic Trigger

Make a small code change.

Example

```
README.md
```

Commit

```bash
git add .

git commit -m "Testing GitHub Webhook"

git push origin main
```

Within a few seconds Jenkins should automatically begin a new build.

No manual action is required.

---

# Step 9 – Validate Console Output

Successful build typically shows

```
Started by GitHub push
```

```
Fetching changes from Git repository
```

```
Checking out Revision
```

```
Building...
```

```
Finished: SUCCESS
```

---

# Step 10 – Verify GitHub Delivery

Open

```
Repository

↓

Settings

↓

Webhooks
```

↓

Click the webhook.

Recent Deliveries should display

```
Response

200 OK
```

Payload Delivered

```
Success
```

---

# Common Issues

---

## Webhook Response 404

Cause

Incorrect webhook URL.

Correct URL

```
http://JENKINS_IP:8080/github-webhook/
```

Verify the trailing slash exists.

---

## Response 403

Cause

Firewall blocks GitHub.

Solution

Allow inbound

```
TCP 8080
```

Security Group example

```
0.0.0.0/0
```

or GitHub IP ranges.

---

## Response Timeout

Cause

Jenkins not reachable.

Verify

```
http://JENKINS_IP:8080
```

opens successfully.

---

## Build Not Triggered

Verify

```
GitHub hook trigger for GITScm polling
```

is enabled.

---

## Repository Authentication Failed

Verify

```
github-ssh
```

credential is selected.

---

## GitHub Delivery Failed

Open

```
Recent Deliveries
```

Click

```
Redeliver
```

Check

```
Response Body
```

for detailed error messages.

---

# Validation Checklist

- Jenkins GitHub Plugin installed
- Git SCM configured
- SSH authentication working
- Webhook created successfully
- Payload URL correct
- Push event selected
- GitHub delivery returns HTTP 200
- Jenkins build starts automatically
- Console Output shows "Started by GitHub push"
- Build completes successfully

---

# Screenshots to Capture

Take screenshots of:

- Jenkins Build Trigger configuration
- GitHub Webhook configuration
- GitHub Recent Deliveries (200 OK)
- Jenkins Build History
- Jenkins Console Output
- Successful automatic pipeline execution
- GitHub commit triggering Jenkins build

---

# Best Practices

- Use HTTPS instead of HTTP
- Configure a Webhook Secret
- Restrict Jenkins access with Security Groups
- Protect the `main` branch
- Enable GitHub Branch Protection Rules
- Use Jenkins Credentials instead of hardcoded secrets
- Use Pipeline as Code (Jenkinsfile)

---

# Skills Demonstrated

- GitHub Webhooks
- Jenkins Automation
- Event-Driven CI/CD
- Git Integration
- SSH Authentication
- Pipeline Triggering
- Continuous Integration
- DevOps Best Practices

---

# Outcome

At the end of this phase:

- Jenkins authenticates with GitHub using SSH.
- GitHub automatically notifies Jenkins after every push.
- Jenkins starts the pipeline automatically.
- Manual "Build Now" is no longer required.
- A complete event-driven Continuous Integration workflow has been implemented.

---

# Next Phase

**Phase 13 – Complete End-to-End Magento CI/CD Pipeline**

Topics covered:

- Jenkinsfile creation
- Multi-stage pipeline
- Docker Compose deployment
- Magento cache flush
- Static content deployment
- Health checks
- Zero-downtime deployment strategy
- Production-ready CI/CD workflow
...
