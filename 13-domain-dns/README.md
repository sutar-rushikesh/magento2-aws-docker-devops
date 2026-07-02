
# Phase 13 - Complete CI/CD Pipeline (Part 1)

# Building a Production-Ready Jenkins Pipeline for Magento 2

---

# Objective

In the previous phases, the following infrastructure was successfully configured:

- AWS EC2 Server
- Docker
- Docker Compose
- Magento 2
- Nginx
- PHP-FPM
- MySQL
- Redis
- OpenSearch
- HTTPS (Let's Encrypt)
- GitHub Repository
- Jenkins Installation
- Jenkins SSH Authentication
- GitHub Webhooks

In this phase, we will build a complete Continuous Integration and Continuous Deployment (CI/CD) pipeline using Jenkins.

The goal is to automate the complete deployment lifecycle so that every code push to GitHub automatically deploys the latest Magento application to the server.

---

# Learning Objectives

By the end of this phase you will learn:

- What is CI/CD
- Jenkins Pipeline Architecture
- Declarative Pipeline
- Jenkinsfile
- Pipeline Stages
- Build Automation
- Docker Deployment
- Deployment Validation
- Automatic Rollback Strategy
- Production Deployment Workflow

---

# What is CI/CD?

Continuous Integration (CI)

Continuous Integration is the practice of automatically integrating code changes into a shared repository whenever developers push new code.

Example

```
Developer

↓

Git Push

↓

GitHub

↓

Jenkins

↓

Build

↓

Testing
```

---

Continuous Deployment (CD)

Continuous Deployment automatically deploys validated builds to the target environment.

```
GitHub

↓

Jenkins Pipeline

↓

Docker Compose

↓

Magento Deployment

↓

Application Live
```

---

# Benefits of CI/CD

- Faster deployments
- Zero manual deployment
- Reduced human error
- Faster feedback
- Improved code quality
- Repeatable deployments
- Production consistency
- Easy rollback
- High availability

---

# Existing Project Architecture

```
Developer

↓

Git Push

↓

GitHub Repository

↓

Webhook

↓

Jenkins

↓

AWS EC2

↓

Docker Compose

↓

Magento Stack
```

---

# Pipeline Architecture

```
Developer

↓

Git Push

↓

GitHub Webhook

↓

Jenkins

↓

Checkout Source Code

↓

Validate Repository

↓

Build Pipeline

↓

Docker Compose

↓

Magento Deployment

↓

Cache Flush

↓

Deployment Validation

↓

Success Notification
```

---

# Pipeline Workflow

```
Stage 1

Checkout Code

↓

Stage 2

Validate Repository

↓

Stage 3

Docker Compose Deployment

↓

Stage 4

Magento Maintenance (Optional)

↓

Stage 5

Cache Flush

↓

Stage 6

Health Check

↓

Stage 7

Deployment Complete
```

---

# CI/CD Workflow

```
Developer

↓

git push

↓

GitHub

↓

Webhook

↓

Jenkins

↓

Clone Repository

↓

Deploy Containers

↓

Restart Services

↓

Magento Cache Flush

↓

Health Check

↓

Production Live
```

---

# Why Use Jenkins Pipelines?

Instead of manually running

```bash
git pull

docker compose down

docker compose up -d

php bin/magento cache:flush

php bin/magento setup:upgrade
```

Everything becomes

```
git push

↓

Automatic Deployment
```

---

# Pipeline Types

Jenkins supports

### Freestyle Project

Simple jobs

Example

```
Execute Shell

↓

Build
```

---

### Declarative Pipeline

Recommended

Pipeline defined as code

```
pipeline {

}
```

Advantages

- Version controlled
- Reusable
- Easy to maintain
- Production standard

---

### Scripted Pipeline

Advanced Groovy scripting

Used for complex workflows.

---

For this project we use

```
Declarative Pipeline
```

---

# Jenkinsfile

The Jenkinsfile is the heart of Jenkins Pipeline.

It defines

- Build steps
- Deployment steps
- Error handling
- Validation
- Notifications

Example

```
pipeline {

agent any

stages {

...

}

}
```

---

# Why Store Jenkinsfile in GitHub?

Infrastructure as Code

Benefits

- Version controlled
- Team collaboration
- Easy rollback
- Easy maintenance
- Reusable

Repository

```
magento2-aws-docker-devops
```

Contains

```
Jenkinsfile
```

---

# Pipeline Stages

Our pipeline consists of

```
Checkout

↓

Validate

↓

Deploy

↓

Magento Commands

↓

Health Check

↓

Finish
```

---

# Stage Overview

## Stage 1

Checkout

Purpose

Download latest source code.

---

## Stage 2

Validate

Purpose

Verify repository contents.

Example

```
docker-compose.yml

exists

Jenkinsfile

exists
```

---

## Stage 3

Deploy

Purpose

Deploy latest Docker containers.

Example

```
docker compose pull

docker compose up -d
```

---

## Stage 4

Magento Maintenance

Purpose

Run Magento maintenance commands.

Examples

```
cache:flush

setup:upgrade

di:compile

indexer:reindex

static-content:deploy
```

Depending on deployment strategy.

---

## Stage 5

Health Check

Purpose

Verify deployment.

Example

```
curl https://pittylittle.shop
```

Expect

```
HTTP 200
```

---

## Stage 6

Deployment Complete

Purpose

Mark deployment successful.

---

# Repository Structure

```
magento2-aws-docker-devops/

│

├── Jenkinsfile

├── docker-compose.yml

├── nginx/

├── php/

├── scripts/

├── README.md

└── screenshots/
```

---

# Jenkins Requirements

Before building the pipeline ensure:

- Jenkins Installed
- Docker Installed
- Docker Compose Installed
- Git Installed
- SSH Credentials Configured
- GitHub Webhook Configured
- Magento Running
- Jenkins User has Docker Permission

---

# Docker Permission

Verify Jenkins can execute Docker commands.

Example

```bash
sudo usermod -aG docker jenkins

sudo systemctl restart jenkins
```

Verify

```bash
sudo su - jenkins

docker ps
```

---

# Environment Variables

Typical variables used

```
REPOSITORY

BRANCH

WORKSPACE

DOCKER_COMPOSE

DOMAIN

MAGENTO_ROOT
```

These improve maintainability.

---

# Security Considerations

Never hardcode

- Passwords
- Tokens
- SSH Keys
- Secrets

Use

```
Jenkins Credentials Manager
```

instead.

---

# Deployment Strategy

Current strategy

```
Docker Compose

↓

Recreate Containers

↓

Application Updated
```

Future enhancement

```
Blue Green Deployment

or

Rolling Update
```

---

# Validation Checklist

Before continuing

- Jenkins Installed
- Docker Installed
- Docker Compose Working
- GitHub Repository Connected
- SSH Authentication Working
- GitHub Webhook Working
- Magento Running
- HTTPS Enabled
- Jenkins User Can Execute Docker

---

# Screenshots to Capture

Take screenshots of

- Jenkins Dashboard
- Existing Pipeline Job
- GitHub Repository
- Jenkins Credentials
- Docker Containers Running
- Magento Homepage
- Jenkins Pipeline Configuration
- Jenkins Workspace

---

# Skills Demonstrated

- Jenkins Pipelines
- Declarative Pipeline
- CI/CD
- Infrastructure as Code
- Docker Automation
- Magento Deployment
- GitHub Integration
- Production Automation
- DevOps Best Practices

---

# Outcome

At the end of this part you understand:

- CI/CD concepts
- Jenkins Pipeline architecture
- Declarative Pipelines
- Jenkinsfile purpose
- Deployment workflow
- Production deployment lifecycle

You are now ready to build the actual production Jenkins pipeline.

---

# Next Phase

## Phase 13 – Part 2

In the next part, we will create the complete **production-ready Jenkinsfile**, including:

- Complete Declarative Pipeline
- Checkout Stage
- Build Stage
- Docker Compose Deployment
- Magento Cache Flush
- Static Content Deployment
- Health Checks
- Post Actions
- Error Handling
- Automatic Deployment Workflow


# Phase 13 – Complete CI/CD Pipeline (Part 2A)

# Creating a Production-Ready Jenkinsfile

---

# Objective

In this part, we will create the complete Jenkins Pipeline that automates the deployment of the Magento 2 application.

The Jenkins pipeline will perform the following tasks automatically:

- Checkout source code
- Verify repository contents
- Deploy Docker containers
- Verify Docker services
- Flush Magento cache
- Validate website availability
- Report deployment status

By the end of this phase, every GitHub push will automatically deploy the latest application.

---

# What is a Jenkinsfile?

A **Jenkinsfile** is a text file stored inside the Git repository that defines the entire CI/CD pipeline.

Instead of manually executing deployment commands, Jenkins reads the Jenkinsfile and performs all deployment steps automatically.

Example:

```
Developer

↓

Git Push

↓

GitHub

↓

Jenkinsfile

↓

Automatic Deployment
```

---

# Advantages

- Pipeline as Code
- Version Controlled
- Easy Collaboration
- Easy Rollback
- Fully Automated
- Production Ready
- Repeatable Deployments

---

# Repository Structure

```
magento2-aws-docker-devops

│

├── Jenkinsfile

├── docker-compose.yml

├── nginx/

├── php/

├── magento/

└── README.md
```

---

# Jenkinsfile Location

Create the Jenkinsfile in the project root.

```
magento2-aws-docker-devops/

│

├── Jenkinsfile
```

---

# Creating Jenkinsfile

```bash
touch Jenkinsfile
```

---

# Complete Production Jenkinsfile

```groovy
pipeline {

    agent any

    environment {

        PROJECT_DIR = "/home/admin/magento-stack"

        DOMAIN = "https://pittylittle.shop"

    }

    options {

        timestamps()

        disableConcurrentBuilds()

    }

    stages {

        stage('Checkout Source') {

            steps {

                echo "Checking out latest source code..."

                checkout scm
            }
        }

        stage('Validate Repository') {

            steps {

                sh '''

                pwd

                ls -lah

                test -f docker-compose.yml

                '''

            }
        }

        stage('Deploy Docker Stack') {

            steps {

                dir("${PROJECT_DIR}") {

                    sh '''

                    docker compose pull

                    docker compose up -d --build

                    '''

                }

            }

        }

        stage('Verify Running Containers') {

            steps {

                sh '''

                docker ps

                '''

            }

        }

        stage('Magento Cache Flush') {

            steps {

                dir("${PROJECT_DIR}") {

                    sh '''

                    docker exec magento-php php bin/magento cache:flush

                    '''

                }

            }

        }

        stage('Website Health Check') {

            steps {

                sh '''

                sleep 15

                curl -I ${DOMAIN}

                '''

            }

        }

    }

    post {

        success {

            echo ""

            echo "===================================="

            echo "Deployment Successful"

            echo "===================================="

        }

        failure {

            echo ""

            echo "===================================="

            echo "Deployment Failed"

            echo "===================================="

        }

        always {

            echo ""

            echo "Pipeline Finished"

        }

    }

}
```

---

# Understanding the Jenkinsfile

The pipeline is divided into multiple sections.

```
pipeline

↓

environment

↓

options

↓

stages

↓

post
```

Each section has a specific purpose.

---

# Pipeline Section

```
pipeline {

}
```

This marks the beginning of the Jenkins Declarative Pipeline.

---

# Agent Section

```
agent any
```

Meaning

Run this pipeline on any available Jenkins executor.

---

# Environment Variables

```
environment {

PROJECT_DIR="/home/admin/magento-stack"

DOMAIN="https://pittylittle.shop"

}
```

Benefits

- No hardcoded values
- Easy maintenance
- Easy migration

---

# Options

```
timestamps()
```

Adds timestamps to Jenkins logs.

Example

```
10:15:22

Deploy Started
```

---

```
disableConcurrentBuilds()
```

Prevents two deployments from running simultaneously.

Without this option

```
Build 15

↓

Build 16

↓

Files overwritten
```

With this option

```
Build 15

↓

Complete

↓

Build 16
```

---

# Pipeline Flow

```
Checkout

↓

Validate

↓

Deploy

↓

Verify

↓

Magento

↓

Health Check

↓

Finish
```

---

# Stage Overview

| Stage | Purpose |
|--------|----------|
| Checkout | Download latest source |
| Validate | Verify repository |
| Deploy | Start Docker containers |
| Verify | Check running containers |
| Magento | Flush cache |
| Health Check | Validate website |

---

# Shell Commands Used

Repository Validation

```bash
pwd

ls -lah

test -f docker-compose.yml
```

---

Docker Deployment

```bash
docker compose pull

docker compose up -d --build
```

---

Verify Containers

```bash
docker ps
```

---

Magento Cache

```bash
docker exec magento-php php bin/magento cache:flush
```

---

Health Check

```bash
curl -I https://pittylittle.shop
```

Expected

```
HTTP/2 200 OK
```

---

# Jenkins Pipeline Execution

```
Pipeline Started

↓

Checkout Source

↓

Repository Validation

↓

Docker Deployment

↓

Verify Containers

↓

Magento Cache Flush

↓

Health Check

↓

Pipeline Success
```

---

# Expected Jenkins Console Output

```
Checking out source...

Repository Validated

Deploying Docker Containers...

Containers Running

Magento Cache Flushed

Website Responding

Deployment Successful
```

---

# Best Practices

✔ Store Jenkinsfile inside Git

✔ Use environment variables

✔ Separate pipeline into stages

✔ Validate before deployment

✔ Verify containers

✔ Perform health checks

✔ Keep logs readable

✔ Avoid hardcoded values

---

# Validation Checklist

Before proceeding ensure:

- Jenkinsfile created
- Stored in repository root
- Syntax correct
- Docker Compose path correct
- Project directory correct
- Domain URL correct
- Docker running
- Magento running

---

# Screenshots to Capture

Take screenshots of:

- Jenkinsfile inside repository
- Jenkins Pipeline configuration
- Jenkinsfile in GitHub
- Pipeline Stage View
- Console Output
- Successful Build
- Docker Containers
- Magento Website

---

# Skills Demonstrated

- Jenkins
- Declarative Pipeline
- Jenkinsfile
- CI/CD
- Docker Compose
- Magento Automation
- Production Deployment
- Infrastructure as Code
- DevOps Automation

---

# Outcome

You have successfully created a production-ready Jenkinsfile that can:

- Automatically deploy code
- Restart Docker services
- Flush Magento cache
- Validate deployment
- Produce build logs
- Support continuous deployment

---

# Next Phase

## Phase 13 – Part 2B

In the next part, we will explain **every stage of the Jenkinsfile line by line**, including:

- Checkout Stage
- Validation Stage
- Deploy Stage
- Docker Commands
- Cache Management
- Health Checks
- Post Actions
- Failure Handling
- Production Best Practices

# Phase 13 – Complete CI/CD Pipeline (Part 2B)

# Understanding the Jenkins Pipeline (Stage by Stage)

---

# Objective

In Part 2A, we created the complete production Jenkinsfile.

In this part, we will understand **every single stage** inside the Jenkins Pipeline.

Rather than blindly copying the Jenkinsfile, a DevOps Engineer should understand:

- Why each stage exists
- What commands are executed
- What happens internally
- How Jenkins communicates with Docker
- How Magento is deployed
- How failures are handled

This is exactly what interviewers expect from an experienced DevOps Engineer.

---

# Pipeline Overview

```
Developer

        │

        ▼

Git Push

        │

        ▼

GitHub Repository

        │

Webhook Trigger

        │

        ▼

Jenkins Pipeline

        │

──────────────────────────────────

Checkout Source

↓

Validate Repository

↓

Deploy Docker Stack

↓

Verify Containers

↓

Magento Cache Flush

↓

Website Health Check

↓

Deployment Successful

──────────────────────────────────

        │

        ▼

Production Website Updated
```

---

# Complete Pipeline Flow

```
GitHub

↓

Webhook

↓

Jenkins

↓

Clone Repository

↓

Docker Compose

↓

Docker Containers

↓

Magento

↓

Website

↓

Health Check

↓

Pipeline Complete
```

---

# Stage 1

# Checkout Source

---

Purpose

Download the latest application source code from GitHub.

Pipeline Code

```groovy
stage('Checkout Source') {

    steps {

        checkout scm

    }

}
```

---

What Happens?

```
GitHub Repository

↓

Clone Latest Commit

↓

Workspace Created

↓

Project Files Available
```

---

Commands Executed

```
git clone

git fetch

git checkout
```

---

Why is this Important?

Without checkout:

```
No Source Code

↓

No Docker Compose

↓

No Jenkinsfile

↓

Deployment Impossible
```

---

Expected Jenkins Console Output

```
Checking out Revision

Fetching upstream changes

Checking out commit

Finished Checkout
```

---

# Stage 2

# Validate Repository

---

Purpose

Verify that the repository contains all required deployment files.

Pipeline

```groovy
stage('Validate Repository') {

steps {

sh '''

pwd

ls -lah

test -f docker-compose.yml

'''

}

}
```

---

Commands Explained

Current directory

```bash
pwd
```

Example

```
/var/lib/jenkins/workspace/magento2
```

---

List repository

```bash
ls -lah
```

Example

```
docker-compose.yml

README.md

nginx/

php/

magento/

Jenkinsfile
```

---

Verify Docker Compose exists

```bash
test -f docker-compose.yml
```

If missing

Pipeline immediately fails.

---

Why Validation Matters

Without validation

```
Pipeline Starts

↓

docker compose

↓

docker-compose.yml Missing

↓

Deployment Failure
```

Validation detects the issue before deployment.

---

# Stage 3

# Deploy Docker Stack

---

Purpose

Deploy the latest Docker containers.

Pipeline

```groovy
stage('Deploy Docker Stack') {

steps {

dir("${PROJECT_DIR}") {

sh '''

docker compose pull

docker compose up -d --build

'''

}

}

}
```

---

Why use dir()?

Repository workspace

```
/var/lib/jenkins/workspace/
```

Magento installation

```
/home/admin/magento-stack
```

Deployment must happen inside the Magento project.

---

docker compose pull

Purpose

Download latest container images.

Example

```
nginx

php

mysql

redis

opensearch
```

---

docker compose up -d --build

Purpose

Rebuild containers if Dockerfile changed.

Restart containers safely.

Flow

```
Stop Old Containers

↓

Rebuild Images

↓

Create Containers

↓

Attach Networks

↓

Mount Volumes

↓

Containers Running
```

---

Expected Console Output

```
Creating Network

Creating Containers

Starting Containers

Done
```

---

# Stage 4

# Verify Running Containers

---

Purpose

Ensure every required service is running.

Pipeline

```groovy
stage('Verify Running Containers') {

steps {

sh '''

docker ps

'''

}

}
```

---

Container Verification

Expected

```
magento-nginx

magento-php

magento-mysql

magento-redis

magento-opensearch
```

---

If one container is missing

Pipeline should fail.

Example

```
php

Exited

↓

Website Down
```

---

Expected Output

```
CONTAINER ID

IMAGE

STATUS

PORTS
```

---

# Stage 5

# Magento Cache Flush

---

Purpose

Remove stale Magento cache.

Pipeline

```groovy
stage('Magento Cache Flush') {

steps {

dir("${PROJECT_DIR}") {

sh '''

docker exec magento-php php bin/magento cache:flush

'''

}

}

}
```

---

Why Flush Cache?

Magento stores

- Configuration
- Layout
- Product Pages
- Blocks
- Static Assets

Without flushing

Users may still see old content.

---

Command

```bash
php bin/magento cache:flush
```

Expected

```
config

layout

block_html

translate

full_page

Flushed Successfully
```

---

# Stage 6

# Website Health Check

---

Purpose

Verify deployment actually succeeded.

Pipeline

```groovy
stage('Website Health Check') {

steps {

sh '''

sleep 15

curl -I https://pittylittle.shop

'''

}

}
```

---

Why Wait?

Containers need time to start.

```
Docker Started

↓

PHP Started

↓

MySQL Ready

↓

Magento Ready

↓

Website Available
```

---

Health Check

```bash
curl -I https://pittylittle.shop
```

Expected

```
HTTP/2 200 OK
```

Failure Example

```
HTTP/2 502

↓

PHP Error

↓

Deployment Failed
```

---

# Why Use curl?

Fast

Reliable

Works in automation

Easy to parse

---

# Stage Dependency

```
Checkout

↓

Validate

↓

Deploy

↓

Verify

↓

Magento Cache

↓

Health Check
```

Every stage depends on the previous one.

---

# Pipeline Failure Example

```
Checkout

✓

↓

Validation

✓

↓

Deploy

✓

↓

Verify

✓

↓

Magento Cache

✓

↓

Health Check

✗

↓

Pipeline Failed
```

---

# Jenkins Stage View

```
Checkout

✔

Validate

✔

Deploy

✔

Verify

✔

Cache

✔

Health

✔
```

---

# Why Small Stages?

Instead of one large deployment script

```
Deploy Everything
```

We split into

```
Checkout

↓

Validate

↓

Deploy

↓

Verify

↓

Cache

↓

Health
```

Benefits

- Easier debugging
- Easier rollback
- Better logging
- Faster issue identification

---

# Common Errors

## Checkout Failure

Reason

Git authentication issue

---

## Validation Failure

Reason

docker-compose.yml missing

---

## Deployment Failure

Reason

Docker daemon stopped

---

## Verification Failure

Reason

Container crashed

---

## Cache Flush Failure

Reason

Magento CLI error

---

## Health Check Failure

Reason

HTTP 500

HTTP 502

Database unavailable

PHP-FPM unavailable

---

# Best Practices

✔ Keep stages small

✔ Validate before deployment

✔ Never skip health checks

✔ Use environment variables

✔ Separate deployment from verification

✔ Use descriptive stage names

✔ Keep logs readable

✔ Fail fast when errors occur

---

# Screenshots to Capture

- Jenkins Stage View
- Console Output for Checkout
- Console Output for Deploy
- Running Docker Containers
- Cache Flush Success
- Website Health Check (HTTP 200)
- Successful Jenkins Build
- Blue Ocean Pipeline (optional)

---

# Skills Demonstrated

- Jenkins Declarative Pipeline
- Git Integration
- Docker Compose Automation
- Container Verification
- Magento CLI Automation
- Website Validation
- CI/CD Workflow Design
- Production Deployment Strategy

---

# Outcome

After completing this phase, you now understand:

- How each Jenkins stage works
- Why every stage exists
- How Docker deployment is automated
- How Magento cache is managed
- How health checks validate deployments
- How Jenkins orchestrates a complete production deployment

---

# Next Phase

## Phase 13 – Part 2C

In the next part, we will cover:

- Post Actions (`success`, `failure`, `always`)
- Notifications
- Rollback Strategy
- Pipeline Optimization
- Security Best Practices
- Production CI/CD Enhancements
- Troubleshooting Failed Deployments
- Real-world Interview Questions


# Phase 13 – Production Deployment
# Part 2 C1 – Magento Production Optimization

---

# Objective

In this phase, we optimize the Magento application for production use.

Magento works in three deployment modes:

- Default
- Developer
- Production

Production mode provides:

- Faster response time
- Precompiled Dependency Injection
- Optimized Static Content
- Better Security
- Lower Resource Consumption
- Improved Customer Experience

This is the recommended mode for all production deployments.

---

# Verify Current Mode

Enter the PHP container.

```bash
docker exec -it magento-php bash
```

Check the deployment mode.

```bash
php bin/magento deploy:mode:show
```

Example output

```
Current application mode: default
```

---

# Switch to Production Mode

Run

```bash
php bin/magento deploy:mode:set production
```

Magento automatically performs several tasks.

Typical output

```
Enabled maintenance mode

Generated classes...

Deploying static content...

Disabling maintenance mode...
```

This process may take several minutes.

---

# Verify Production Mode

Run

```bash
php bin/magento deploy:mode:show
```

Expected output

```
Current application mode: production
```

---

# Compile Dependency Injection

Compile Magento generated classes.

```bash
php bin/magento setup:di:compile
```

Expected output

```
Compilation was started.

Repositories code generation...

Interceptors generation...

Generated code and dependency injection configuration successfully.
```

---

# Deploy Static Content

Generate CSS, JavaScript, Fonts, Images and Themes.

```bash
php bin/magento setup:static-content:deploy -f
```

Or for English locale

```bash
php bin/magento setup:static-content:deploy -f en_US
```

Example

```
frontend/Magento/luma/en_US

100%

adminhtml/Magento/backend/en_US

100%
```

---

# Flush Magento Cache

```bash
php bin/magento cache:flush
```

Output

```
Flushed cache types successfully.
```

---

# Reindex Magento

```bash
php bin/magento indexer:reindex
```

Example output

```
Design Config Grid index has been rebuilt successfully.

Product Categories index rebuilt successfully.

Catalog Search index rebuilt successfully.
```

---

# Verify Index Status

```bash
php bin/magento indexer:status
```

Expected

```
Ready

Ready

Ready

Ready
```

Every index should display **Ready**.

---

# Verify Static Files

Check generated CSS.

```bash
find pub/static/frontend -name styles-m.css
```

Example

```
pub/static/frontend/Magento/luma/en_US/css/styles-m.css
```

---

# Verify Production Website

Open

```
https://your-domain.com
```

Verify

- Homepage loads successfully
- CSS loads
- JavaScript loads
- Images load
- Products display correctly
- Search works
- Admin Panel opens
- No missing static resources

---

# Verify Generated Static Assets

Example URL

```
https://your-domain.com/static/versionxxxxxxxx/frontend/Magento/luma/en_US/css/styles-m.css
```

Expected

```
HTTP/2 200 OK
```

---

# Check Cache Status

```bash
php bin/magento cache:status
```

Expected

```
config : Enabled

layout : Enabled

block_html : Enabled

collections : Enabled

translate : Enabled

full_page : Enabled
```

---

# Useful Commands

Show deployment mode

```bash
php bin/magento deploy:mode:show
```

Enable production mode

```bash
php bin/magento deploy:mode:set production
```

Compile

```bash
php bin/magento setup:di:compile
```

Deploy static files

```bash
php bin/magento setup:static-content:deploy -f
```

Reindex

```bash
php bin/magento indexer:reindex
```

Flush cache

```bash
php bin/magento cache:flush
```

---

# Troubleshooting

## Static files missing

Run

```bash
rm -rf pub/static/*
rm -rf var/view_preprocessed/*
php bin/magento setup:static-content:deploy -f
```

---

## CSS not loading

Verify

```
pub/static
```

Verify

```
Nginx configuration
```

Flush cache

---

## Website showing 502

Check

```bash
docker logs magento-nginx
```

Check

```bash
docker logs magento-php
```

Verify PHP-FPM is running.

---

## Indexers not Ready

Run

```bash
php bin/magento indexer:reindex
```

---

# Best Practices

✔ Always use Production Mode on live servers.

✔ Compile Dependency Injection after installing new modules.

✔ Redeploy static content after theme changes.

✔ Flush cache after configuration changes.

✔ Monitor indexer status regularly.

✔ Keep generated content synchronized during deployments.

✔ Automate these commands in CI/CD pipelines.

---

# Outcome

After completing this phase:

- Magento runs in Production Mode.
- Dependency Injection is compiled.
- Static assets are generated.
- Cache is optimized.
- Indexers are synchronized.
- Website performance is significantly improved.
- The application is ready for production traffic.

---

**Next Phase:** Phase 13 – Part 2 C2 (Nginx Production Configuration & Performance Tuning)
# Phase 13 – Production Deployment
# Part 2C2 – Nginx Production Configuration & Performance Optimization

---

# Objective

In this phase, we optimize the Nginx web server for running Magento 2 in a production environment.

Magento is a high-performance PHP application that requires proper Nginx configuration for:

- SSL Support
- Static Content Delivery
- PHP-FPM Communication
- FastCGI Buffer Optimization
- Media File Handling
- Security Hardening
- Better Performance

By the end of this phase, the Magento application will be served securely over HTTPS with optimized Nginx settings.

---

# Why Nginx?

Nginx is one of the most widely used web servers because it offers:

- High Performance
- Low Memory Usage
- Reverse Proxy Support
- SSL Termination
- Static File Caching
- Load Balancing
- FastCGI Integration

---

# Architecture

```

Internet
│
▼
HTTPS (443)
│
▼
Nginx
│
▼
PHP-FPM
│
▼
Magento 2
│
▼
MySQL / Redis / OpenSearch

```

---

# Nginx Responsibilities

Nginx performs the following tasks:

- Accept HTTPS Requests
- Redirect HTTP to HTTPS
- Serve Static Files
- Forward PHP Requests
- Handle SSL Certificates
- Communicate with PHP-FPM
- Protect Sensitive Files

---

# Production Nginx Configuration

Location

```

nginx/default.conf

```

The configuration contains two server blocks:

```

HTTP
↓

Redirect

↓

HTTPS

↓

Magento

```

---

# HTTP Redirect

All HTTP requests should redirect to HTTPS.

```
server {

listen 80;

server_name pittylittle.shop www.pittylittle.shop;

return 301 https://$host$request_uri;

}
```

Benefits

- Secure communication
- Better SEO
- PCI Compliance

---

# HTTPS Server

```
server {

listen 443 ssl;

http2 on;

}
```

Purpose

- Enable SSL
- Enable HTTP/2

---

# SSL Certificates

```
ssl_certificate

ssl_certificate_key
```

Certificates are mounted from

```
/etc/letsencrypt
```

Benefits

- Encrypted Communication
- Browser Trust
- Secure Payments

---

# Magento Root Directory

```
root /var/www/html/pub;
```

Magento serves everything through

```
pub/
```

This prevents exposing application files.

---

# Index File

```
index index.php;
```

Nginx first searches

```
index.php
```

---

# Client Upload Size

```
client_max_body_size 64M;
```

Allows

- Product Images
- CSV Imports
- Extensions
- Media Uploads

---

# Main Location Block

```
location / {

try_files

}
```

Purpose

```
Check Requested File

↓

If Exists

↓

Serve Directly

↓

Else

↓

Pass to Magento
```

---

# Static Content

```
location /static/
```

Purpose

Serve

- CSS
- JS
- Fonts
- Images

without invoking PHP.

---

# Static Content Cache

```
expires max;

access_log off;
```

Benefits

- Browser Caching
- Faster Loading
- Reduced Server Load

---

# Media Files

```
location /media/
```

Serves

- Product Images
- Category Images
- Customer Uploads

---

# PHP Processing

```
location ~ \.php$
```

Forwards PHP files to

```
PHP-FPM
```

---

# FastCGI

```
fastcgi_pass php:9000;
```

Docker service

```
php
```

Port

```
9000
```

---

# PHP Entry Point

```
fastcgi_index index.php;
```

---

# Script Filename

```
fastcgi_param SCRIPT_FILENAME

```

Maps requested URL

to

```
Actual PHP File
```

---

# HTTPS Parameter

```
fastcgi_param HTTPS on;
```

Magento correctly detects

```
HTTPS Request
```

Without this

- Mixed Content
- Redirect Loops
- Login Issues

may occur.

---

# FastCGI Read Timeout

```
fastcgi_read_timeout 600;
```

Magento operations

- Compilation
- Cache
- Imports

can take longer.

600 seconds prevents premature timeout.

---

# FastCGI Buffer Optimization

This was one of the major production issues encountered during deployment.

Original error

```
upstream sent too big header while reading response header from upstream
```

Cause

Magento generates large response headers due to:

- Cookies
- Cache Headers
- Sessions
- Customer Data

---

# Solution

Increase FastCGI Buffers.

```
fastcgi_buffer_size 128k;

fastcgi_buffers 16 64k;

fastcgi_busy_buffers_size 256k;

fastcgi_temp_file_write_size 256k;
```

Result

```
HTTP 502

↓

Resolved

↓

HTTP 200
```

---

# Security Block

```
location ~ /\.(?!well-known).* {

deny all;

}
```

Blocks access to

```
.env

.git

.htaccess

composer.json

app/etc

```

Improves server security.

---

# Validate Configuration

Run

```
docker exec magento-nginx nginx -t
```

Expected

```
syntax is ok

test is successful
```

---

# Reload Nginx

```
docker exec magento-nginx nginx -s reload
```

No container restart required.

---

# Verify Homepage

```
curl -I https://pittylittle.shop
```

Expected

```
HTTP/2 200
```

---

# Verify Static Files

```
curl -I https://pittylittle.shop/static/versionXXXX/frontend/Magento/luma/en_US/css/styles-m.css
```

Expected

```
HTTP/2 200
```

---

# Verify SSL

Open

```
https://pittylittle.shop
```

Browser should display

```
Secure Connection
```

---

# Verify Containers

```
docker ps
```

Expected

```
magento-nginx

magento-php

magento-mysql

magento-redis

magento-opensearch
```

---

# Common Errors

## 502 Bad Gateway

Reason

```
PHP-FPM unavailable
```

or

```
FastCGI Buffers Too Small
```

Solution

Increase

```
fastcgi_buffers
```

---

## Static Files 404

Solution

```
php bin/magento setup:static-content:deploy -f
```

---

## Mixed Content

Verify

```
web/secure/base_url
```

---

## SSL Issues

Verify

```
Let's Encrypt

Certificate

Expiry
```

---

# Performance Optimizations Applied

✔ HTTP to HTTPS Redirect

✔ HTTP/2 Enabled

✔ Browser Caching

✔ Static Content Optimization

✔ FastCGI Buffer Tuning

✔ Long PHP Timeout

✔ Secure Root Directory

✔ Hidden File Protection

✔ HTTPS Detection

✔ Optimized Static Resource Delivery

---

# Best Practices

- Always serve Magento from `/pub`
- Never expose application root
- Use HTTPS only
- Enable browser caching
- Tune FastCGI buffers
- Disable access logs for static files
- Protect hidden files
- Validate Nginx configuration before reload
- Monitor Nginx logs regularly

---

# Screenshots to Capture

Take screenshots of:

- Nginx Configuration (`default.conf`)
- `docker exec magento-nginx nginx -t`
- `docker exec magento-nginx nginx -s reload`
- Browser showing HTTPS lock
- `curl -I https://pittylittle.shop`
- `curl` for static CSS (HTTP 200)
- Magento homepage
- Docker containers running
- Browser Developer Tools → Network tab showing CSS loaded successfully

Store them under:

```
screenshots/phase13-part2c2/
```

---

# Learning Outcomes

After completing this phase you have learned:

- Nginx Reverse Proxy Configuration
- HTTPS Configuration
- Let's Encrypt Integration
- PHP-FPM Integration
- FastCGI Optimization
- Static Content Delivery
- Browser Caching
- Security Hardening
- Magento Production Configuration
- Troubleshooting 502 Bad Gateway
- Performance Tuning

---

# Outcome

At the end of this phase:

- Magento is served securely over HTTPS.
- Nginx is optimized for production.
- FastCGI communication is tuned for Magento.
- Static content loads successfully.
- HTTP/2 is enabled.
- The "upstream sent too big header" issue has been resolved.
- The application is production-ready with optimized web server performance.

---

## Next Phase

➡ **Phase 13 – Part 3: Final Production Validation, End-to-End Testing & GitHub Repository Publishing**
...
