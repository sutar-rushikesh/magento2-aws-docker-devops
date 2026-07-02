
# 🔐 Phase 11 — Domain Configuration, SSL (Let's Encrypt), HTTPS & Final Production Validation

## 🎯 Objective

After optimizing Magento for production, the final step is to make the application publicly accessible over a secure HTTPS connection.

In this phase, we will:

- Purchase and configure a custom domain
- Configure DNS records in GoDaddy
- Point the domain to the AWS EC2 instance
- Install SSL certificates using Let's Encrypt
- Configure Nginx for HTTPS
- Update Magento Base URLs
- Verify SSL, HTTP/2, and secure static assets
- Perform complete end-to-end production validation

By the end of this phase, Magento will be running securely on a custom domain with HTTPS enabled.

---

# Architecture

```

                Internet
                     │
          pittylittle.shop
                     │
            GoDaddy DNS Records
                     │
              AWS Elastic IP
                     │
               Ubuntu EC2 Server
                     │
             Docker Compose Stack
                     │
              ┌──────────────┐
              │    Nginx     │
              └──────┬───────┘
                     │
               PHP-FPM (Docker)
                     │
                  Magento 2
                     │
       ┌────────┬────────┬─────────┐
       │        │        │         │
    MySQL    Redis  OpenSearch  HTTPS

```

---

# Prerequisites

Before proceeding, ensure:

- Magento is installed
- Production Mode is enabled
- Docker containers are running
- Nginx is configured
- EC2 Security Group allows:
  - Port 80
  - Port 443
- Domain purchased from GoDaddy

---

# Step 1 — Purchase a Domain

Purchase a domain from any registrar.

Example:

```
pittylittle.shop
```

---

# Step 2 — Allocate an Elastic IP

AWS Console

```
EC2

↓

Elastic IP

↓

Allocate Elastic IP

↓

Associate with EC2
```

Copy the Elastic IP.

Example

```
34.xxx.xxx.xxx
```

---

# Step 3 — Configure GoDaddy DNS

Open

```
GoDaddy

↓

DNS Management
```

Create the following records.

---

## Root Domain

| Type | Name | Value |
|------|------|--------|
| A | @ | Elastic IP |

---

## WWW

| Type | Name | Value |
|------|------|--------|
| CNAME | www | @ |

Wait for DNS propagation.

---

# Step 4 — Verify DNS

Run

```bash
nslookup pittylittle.shop
```

Expected

```
Address:

Elastic IP
```

---

Verify

```bash
ping pittylittle.shop
```

---

# Step 5 — Install Certbot

```bash
sudo apt update
```

```bash
sudo apt install certbot -y
```

---

# Step 6 — Stop Docker Nginx Temporarily

Since Certbot uses port 80.

```bash
docker compose stop nginx
```

---

# Step 7 — Generate SSL Certificate

```bash
sudo certbot certonly --standalone \
-d pittylittle.shop \
-d www.pittylittle.shop
```

Expected

```
Congratulations!

Your certificate has been saved.
```

Certificates stored at

```
/etc/letsencrypt/live/pittylittle.shop/
```

---

# Step 8 — Verify SSL Files

```bash
ls /etc/letsencrypt/live/pittylittle.shop/
```

Expected

```
fullchain.pem

privkey.pem
```

---

# Step 9 — Mount SSL Certificates into Docker

Update docker-compose.yml

```yaml
volumes:
  - /etc/letsencrypt:/etc/letsencrypt:ro
```

Restart containers

```bash
docker compose up -d
```

---

# Step 10 — Configure Nginx

HTTP Redirect

```nginx
server {

listen 80;

server_name pittylittle.shop www.pittylittle.shop;

return 301 https://$host$request_uri;

}
```

HTTPS Server

```nginx
server {

listen 443 ssl;
http2 on;

server_name pittylittle.shop www.pittylittle.shop;

ssl_certificate /etc/letsencrypt/live/pittylittle.shop/fullchain.pem;

ssl_certificate_key /etc/letsencrypt/live/pittylittle.shop/privkey.pem;

}
```

---

# Step 11 — Validate Nginx

```bash
docker exec magento-nginx nginx -t
```

Expected

```
syntax is ok
```

Reload

```bash
docker exec magento-nginx nginx -s reload
```

---

# Step 12 — Update Magento Base URLs

Secure URL

```bash
docker exec -it magento-php php bin/magento setup:store-config:set \
--base-url=https://pittylittle.shop/
```

---

Update Unsecure URL

```bash
docker exec -it magento-php php bin/magento config:set web/unsecure/base_url https://pittylittle.shop/
```

---

Update Secure URL

```bash
docker exec -it magento-php php bin/magento config:set web/secure/base_url https://pittylittle.shop/
```

---

Enable HTTPS

```bash
docker exec -it magento-php php bin/magento config:set web/secure/use_in_frontend 1
```

```bash
docker exec -it magento-php php bin/magento config:set web/secure/use_in_adminhtml 1
```

Flush Cache

```bash
docker exec -it magento-php php bin/magento cache:flush
```

---

# Step 13 — Verify Base URLs

```bash
docker exec -it magento-php php bin/magento config:show web/secure/base_url
```

Expected

```
https://pittylittle.shop/
```

---

# Step 14 — Verify Homepage

Open

```
https://pittylittle.shop
```

Expected

Magento Homepage

---

# Step 15 — Verify Admin

```
https://pittylittle.shop/admin_xxxxxx
```

Login successfully.

---

# Step 16 — Verify SSL

Run

```bash
curl -I https://pittylittle.shop
```

Expected

```
HTTP/2 200
```

---

Verify Certificate

Open browser

```
🔒 Lock Icon

↓

Certificate Valid
```

---

# Step 17 — Verify HTTP Redirect

```bash
curl -I http://pittylittle.shop
```

Expected

```
301

↓

https://pittylittle.shop
```

---

# Step 18 — Verify Static Assets

```bash
curl -I https://pittylittle.shop/static/frontend/Magento/luma/en_US/css/styles-m.css
```

Expected

```
HTTP/2 200
```

---

# Step 19 — Verify Media

```bash
curl -I https://pittylittle.shop/media/logo/default/logo.svg
```

Expected

```
HTTP/2 200
```

---

# Step 20 — Verify SSL Grade

Visit

```
https://www.ssllabs.com/ssltest/
```

Expected

```
A+

or

A
```

---

# Step 21 — Verify HTTP/2

Browser

Developer Tools

↓

Network

↓

Protocol

Expected

```
h2
```

---

# Step 22 — Final Validation

Verify

✅ Homepage

✅ Admin

✅ Products

✅ Images

✅ CSS

✅ JS

✅ Search

✅ Cart

✅ HTTPS

✅ SSL

---

# Useful Commands

Check SSL

```bash
curl -I https://pittylittle.shop
```

Reload Nginx

```bash
docker exec magento-nginx nginx -s reload
```

Check Logs

```bash
docker logs magento-nginx
```

Flush Cache

```bash
php bin/magento cache:flush
```

---

# Common Issues

## Mixed Content

Flush cache

```bash
php bin/magento cache:flush
```

---

## SSL Not Loading

Verify

```
fullchain.pem

privkey.pem
```

---

## CSS Missing

Deploy

```bash
php bin/magento setup:static-content:deploy -f
```

---

## HTTPS Redirect Loop

Verify

```
web/secure/base_url

web/unsecure/base_url
```

---

## DNS Not Working

Wait

```
15–60 minutes
```

---

# Best Practices

✔ Use Elastic IP

✔ Enable HTTPS

✔ Redirect HTTP → HTTPS

✔ Use HTTP/2

✔ Use Let's Encrypt

✔ Use Strong SSL Cipher Suites

✔ Configure Auto-Renewal

✔ Enable Browser Caching

✔ Secure Admin URL

✔ Regular Certificate Renewal

---

# Validation Checklist

| Validation | Status |
|------------|--------|
| Domain Purchased | ✅ |
| Elastic IP Configured | ✅ |
| DNS Configured | ✅ |
| SSL Installed | ✅ |
| HTTPS Working | ✅ |
| HTTP Redirect Working | ✅ |
| Magento Base URLs Updated | ✅ |
| Homepage Working | ✅ |
| Admin Working | ✅ |
| HTTP/2 Enabled | ✅ |
| Static Files Loading | ✅ |
| SSL Verified | ✅ |

---

# Screenshots

Capture the following screenshots.

---

## GoDaddy DNS Records

```
screenshots/godaddy-dns.png
```

```markdown
![GoDaddy DNS](../screenshots/godaddy-dns.png)
```

---

## AWS Elastic IP

```
screenshots/aws-elastic-ip.png
```

```markdown
![Elastic IP](../screenshots/aws-elastic-ip.png)
```

---

## Certbot Installation

```
screenshots/certbot-install.png
```

```markdown
![Certbot](../screenshots/certbot-install.png)
```

---

## SSL Certificate Generated

```
screenshots/ssl-generated.png
```

```markdown
![SSL Generated](../screenshots/ssl-generated.png)
```

---

## Nginx HTTPS Configuration

```
screenshots/nginx-ssl-config.png
```

```markdown
![Nginx SSL](../screenshots/nginx-ssl-config.png)
```

---

## Magento Base URL

```
screenshots/base-url.png
```

```markdown
![Base URL](../screenshots/base-url.png)
```

---

## HTTPS Homepage

```
screenshots/https-homepage.png
```

```markdown
![Homepage HTTPS](../screenshots/https-homepage.png)
```

---

## SSL Lock Icon

```
screenshots/ssl-lock.png
```

```markdown
![SSL Lock](../screenshots/ssl-lock.png)
```

---

## Magento Admin HTTPS

```
screenshots/admin-https.png
```

```markdown
![Admin HTTPS](../screenshots/admin-https.png)
```

---

## HTTP/2 Verification

```
screenshots/http2.png
```

```markdown
![HTTP2](../screenshots/http2.png)
```

---

## SSL Labs Report

```
screenshots/ssl-labs.png
```

```markdown
![SSL Labs](../screenshots/ssl-labs.png)
```

---

# Skills Demonstrated

By completing this phase, you have demonstrated the ability to:

- Configure DNS records for a production domain
- Associate an AWS Elastic IP with an EC2 instance
- Install and manage Let's Encrypt SSL certificates
- Configure Nginx for HTTPS and HTTP/2
- Update Magento Base URLs for secure access
- Validate HTTPS, SSL certificates, and secure static assets
- Troubleshoot common SSL, DNS, and HTTPS issues
- Deploy a publicly accessible, secure Magento application following production best practices

---

# Final Outcome

Congratulations! 🎉

You have successfully completed a **production-grade Magento 2 deployment on AWS using Docker Compose**.

Your deployment now includes:

- Dockerized Magento 2
- Nginx Reverse Proxy
- PHP-FPM
- MySQL
- Redis
- OpenSearch
- Docker Networking
- Production Mode
- Static Content Deployment
- Dependency Injection Compilation
- Cron Jobs
- Custom Domain
- Let's Encrypt SSL
- HTTPS & HTTP/2
- Secure Public Access

This project demonstrates a complete real-world DevOps implementation suitable for portfolio projects, interviews, and production deployments.

---

# 🚀 Next Phase

**Phase 12 — CI/CD Pipeline with Jenkins & GitHub**

In the next phase, we will automate the deployment process by:

- Integrating GitHub with Jenkins
- Building Docker images automatically
- Deploying updated containers on code changes
- Automating Magento cache flush and static content deployment
- Creating a production-ready CI/CD pipeline
...
