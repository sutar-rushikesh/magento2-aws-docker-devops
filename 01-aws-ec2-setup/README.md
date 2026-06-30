# Phase 01 — AWS EC2 Setup

## Objective

Provision an Ubuntu EC2 instance on AWS that will host the Magento 2 Docker environment.

---

# Services Used

- AWS EC2
- AWS VPC
- Security Groups
- Elastic IP (Optional)
- SSH

---

# Instance Configuration

| Setting | Value |
|----------|-------|
| Instance Type | t3.large |
| Operating System | Ubuntu 24.04 LTS |
| Architecture | x86_64 |
| Storage | 30 GB gp3 |
| Region | us-east-1 |
| Key Pair | assessment.pem |

---

# Why t3.large?

Magento requires multiple services:

- PHP
- MySQL
- Redis
- OpenSearch
- Nginx

A t3.large provides:

- 2 vCPU
- 8 GB RAM

which is sufficient for a production demo.

---

# Security Group Configuration

| Port | Protocol | Purpose |
|-------|----------|----------|
| 22 | TCP | SSH |
| 80 | TCP | HTTP |
| 443 | TCP | HTTPS |

---

# Launch Steps

1. Login to AWS Console

2. Navigate to

```
EC2
```

3. Click

```
Launch Instance
```

4. Configure

- Ubuntu 24.04 LTS
- t3.large
- 30 GB Storage
- Existing Key Pair
- Security Group

5. Launch instance

---

# Verify SSH Connectivity

```bash
ssh -i assessment.pem ubuntu@<EC2-Public-IP>
```

Example

```bash
ssh -i assessment.pem admin@34.xxx.xxx.xxx
```

---

# Validation

Verify

```bash
hostname
```

```bash
lsb_release -a
```

```bash
df -h
```

```bash
free -h
```

---

# Expected Output

- Ubuntu Login Successful
- Internet Connectivity
- Storage Available
- RAM Available

---

# Screenshots

Add the following screenshots.

```
screenshots/

aws-instance-created.png

aws-security-group.png

aws-instance-running.png

ssh-login.png
```

Example

```markdown
## EC2 Instance

![EC2](../screenshots/aws-instance-created.png)

---

## Security Group

![Security Group](../screenshots/aws-security-group.png)

---

## SSH Login

![SSH](../screenshots/ssh-login.png)
```

---

# Outcome

✔ Ubuntu EC2 server created

✔ SSH connectivity established

✔ Server ready for Docker installation
