
# 🌐 Phase 08 — Configuring Nginx for Magento 2 (Production Ready)

## 🎯 Objective

In the previous phase, we created a custom PHP-FPM Docker image optimized for Magento 2.

In this phase, we will configure **Nginx** as the front-end web server to serve Magento in a production environment.

By the end of this phase, you will have a production-ready Nginx configuration that:

- Serves Magento efficiently
- Connects with PHP-FPM using FastCGI
- Delivers static content efficiently
- Serves media files
- Supports HTTPS
- Improves performance
- Increases security

---

# Why Nginx?

Magento officially recommends **Nginx** for production deployments because it offers:

- High Performance
- Low Memory Usage
- Excellent Static File Handling
- Fast Reverse Proxy
- SSL Support
- HTTP/2
- Better Scalability

---

# Architecture

```
                  Internet
                       │
                HTTPS (443)
                       │
                ┌────────────┐
                │   Nginx    │
                └─────┬──────┘
                      │
              FastCGI Port 9000
                      │
                ┌────────────┐
                │ PHP-FPM    │
                └────────────┘
                      │
                 Magento 2
```

---

# Nginx Responsibilities

Nginx performs several critical tasks.

- Accepts HTTP/HTTPS requests
- Redirects HTTP to HTTPS
- Serves Static Content
- Serves Media Files
- Passes PHP Requests to PHP-FPM
- Protects Sensitive Files
- Adds Cache Headers
- Improves Performance

---

# Project Structure

```
magento-stack/

├── nginx
│
├── default.conf
│
└── ssl
```

---

# Nginx Configuration File

```
nginx/default.conf
```

This file contains all server configurations.

---

# HTTP Server Block

Port 80 redirects every request to HTTPS.

Example

```nginx
server {

    listen 80;

    server_name example.com www.example.com;

    return 301 https://$host$request_uri;

}
```

Why?

- Forces HTTPS
- Better SEO
- Secure communication

---

# HTTPS Server Block

Example

```nginx
server {

    listen 443 ssl;

    server_name example.com;

}
```

This server handles all secure requests.

---

# SSL Certificates

Example

```nginx
ssl_certificate

ssl_certificate_key
```

These files are generated using Let's Encrypt.

---

# Document Root

```nginx
root /var/www/html/pub;
```

Magento should always point to:

```
pub/
```

Never expose the Magento root directory.

---

# Index File

```nginx
index index.php;
```

Default entry point.

---

# Client Upload Size

```nginx
client_max_body_size 64M;
```

Allows larger product image uploads.

---

# Main Location Block

```nginx
location / {

    try_files $uri $uri/ /index.php?$args;

}
```

Purpose

- Serves existing files
- Falls back to Magento routing

---

# Static Content

```nginx
location /static/ {

    expires max;

    access_log off;

}
```

Purpose

- Cache CSS
- Cache JS
- Cache Fonts
- Cache Images

Improves performance significantly.

---

# Static Content Fallback

Magento generates static content dynamically.

```nginx
try_files

/static.php
```

If file doesn't exist

↓

Magento generates it automatically.

---

# Media Files

```nginx
location /media/

```

Purpose

Serve

- Product Images
- Category Images
- Uploaded Files

---

# Media Cache

```nginx
expires max;
```

Browsers cache images for a long period.

Improves page load speed.

---

# PHP Processing

```nginx
location ~ \.php$
```

Every PHP request is forwarded to PHP-FPM.

---

# FastCGI

```nginx
fastcgi_pass php:9000;
```

Nginx communicates with the PHP container.

---

# FastCGI Index

```nginx
fastcgi_index index.php;
```

Default PHP file.

---

# FastCGI Parameters

```nginx
include fastcgi_params;
```

Passes required environment variables.

---

# Script Filename

```nginx
fastcgi_param SCRIPT_FILENAME
```

Tells PHP which file to execute.

---

# HTTPS Variable

```nginx
fastcgi_param HTTPS on;
```

Allows Magento to detect HTTPS correctly.

---

# FastCGI Timeout

```nginx
fastcgi_read_timeout 600;
```

Magento operations can be lengthy.

This prevents timeout errors.

---

# FastCGI Buffers

```nginx
fastcgi_buffer_size

fastcgi_buffers

fastcgi_busy_buffers_size

fastcgi_temp_file_write_size
```

Purpose

Avoid

```
502 Bad Gateway

upstream sent too big header
```

These settings are extremely important for Magento.

---

# Protect Hidden Files

```nginx
location ~ /\.(?!well-known)
```

Blocks access to

```
.env

.git

.htaccess

composer.json

composer.lock
```

Improves security.

---

# Verify Nginx Configuration

Run

```bash
docker exec magento-nginx nginx -t
```

Expected

```
syntax is ok

test is successful
```

---

# Reload Configuration

```bash
docker exec magento-nginx nginx -s reload
```

No restart required.

---

# Verify Homepage

```bash
curl -I https://yourdomain.com
```

Expected

```
HTTP/2 200
```

---

# Verify Static Content

```bash
curl -I https://yourdomain.com/static/frontend/...
```

Expected

```
HTTP/2 200
```

---

# Verify Media

```bash
curl -I https://yourdomain.com/media/...
```

Expected

```
HTTP/2 200
```

---

# Verify PHP

```bash
curl https://yourdomain.com
```

Homepage should load successfully.

---

# Verify SSL

```bash
curl -Iv https://yourdomain.com
```

Expected

```
SSL certificate verify ok
```

---

# Useful Commands

Reload Nginx

```bash
docker exec magento-nginx nginx -s reload
```

Check configuration

```bash
docker exec magento-nginx nginx -t
```

View logs

```bash
docker logs magento-nginx
```

Restart

```bash
docker restart magento-nginx
```

Enter container

```bash
docker exec -it magento-nginx sh
```

---

# Common Issues

## 502 Bad Gateway

Cause

- PHP container down
- FastCGI misconfigured
- PHP-FPM not listening

Check

```bash
docker logs magento-nginx
```

---

## CSS Not Loading

Cause

Incorrect Static Configuration

Redeploy

```bash
php bin/magento setup:static-content:deploy -f
```

---

## Media Not Loading

Verify

```bash
/media/
```

configuration.

---

## SSL Errors

Check

```bash
docker logs magento-nginx
```

Verify certificate paths.

---

## Redirect Loop

Usually caused by

```
Incorrect Base URL

HTTPS Configuration
```

Verify

```bash
php bin/magento config:show web/secure/base_url
```

---

# Best Practices Implemented

✔ HTTPS Redirect

✔ HTTP/2

✔ PHP-FPM

✔ Static Content Caching

✔ Media Caching

✔ FastCGI Buffers

✔ Secure File Access

✔ Optimized Timeouts

✔ Docker Networking

✔ Production Ready

---

# Validation Checklist

| Validation | Status |
|------------|--------|
| Nginx Installed | ✅ |
| HTTP Redirect | ✅ |
| HTTPS Working | ✅ |
| SSL Configured | ✅ |
| PHP Connected | ✅ |
| Static Files Working | ✅ |
| Media Files Working | ✅ |
| FastCGI Working | ✅ |
| Homepage Accessible | ✅ |
| Production Ready | ✅ |

---

# Screenshots

Capture the following screenshots.

---

## Nginx Configuration

```
screenshots/nginx-default-conf.png
```

```markdown
![Nginx Config](../screenshots/nginx-default-conf.png)
```

---

## Nginx Test

```
screenshots/nginx-test.png
```

```markdown
![Nginx Test](../screenshots/nginx-test.png)
```

---

## Nginx Reload

```
screenshots/nginx-reload.png
```

```markdown
![Nginx Reload](../screenshots/nginx-reload.png)
```

---

## HTTPS Homepage

```
screenshots/magento-homepage.png
```

```markdown
![Magento Homepage](../screenshots/magento-homepage.png)
```

---

## Static CSS (HTTP 200)

```
screenshots/static-css.png
```

```markdown
![Static CSS](../screenshots/static-css.png)
```

---

## Media File

```
screenshots/media-file.png
```

```markdown
![Media File](../screenshots/media-file.png)
```

---

## Nginx Logs

```
screenshots/nginx-logs.png
```

```markdown
![Nginx Logs](../screenshots/nginx-logs.png)
```

---

# Skills Demonstrated

After completing this phase, you can confidently:

- Configure Nginx for Magento 2
- Configure HTTPS using SSL certificates
- Configure FastCGI with PHP-FPM
- Serve static and media content efficiently
- Optimize Nginx for production workloads
- Secure the web server by restricting sensitive file access
- Troubleshoot common Nginx and Magento integration issues
- Validate and reload Nginx configurations without downtime

---

# Final Outcome

At the end of this phase, you have successfully configured **Nginx as a production-ready web server** for Magento 2.

The server is capable of:

- Handling secure HTTPS traffic
- Communicating with PHP-FPM
- Serving Magento storefront pages
- Delivering static assets and media efficiently
- Applying caching and performance optimizations
- Protecting sensitive application files

This completes the web server layer of the Magento deployment stack.

---

# 🚀 Next Phase

**Phase 09 — Installing Magento 2 Using Composer**

In the next phase, we will:

- Install Composer dependencies
- Download Magento source code
- Configure database connectivity
- Connect Redis and OpenSearch
- Execute the Magento installation command
- Configure admin credentials
- Validate the storefront and admin panel
- Switch Magento to Production Mode
...
