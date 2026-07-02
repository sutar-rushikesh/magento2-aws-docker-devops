
# ⚡ Phase 10 — Production Optimization (Redis, OpenSearch, Production Mode & Performance Tuning)

## 🎯 Objective

After successfully installing Magento 2, the application is functional but not yet optimized for production workloads.

In this phase, we will configure and verify all production components to ensure Magento delivers high performance, efficient caching, optimized search capabilities, and reliable background processing.

By the end of this phase, the Magento store will be fully optimized for production.

---

# Architecture

```

                    Internet
                         │
                   HTTPS (443)
                         │
                  ┌─────────────┐
                  │    Nginx    │
                  └──────┬──────┘
                         │
                    PHP-FPM 8.3
                         │
          ┌──────────────┼───────────────┐
          │              │               │
      MySQL          Redis Cache    OpenSearch
          │              │               │
          └────────── Magento 2 ─────────┘

```

---

# Production Optimizations

This phase includes the following tasks:

- Enable Production Mode
- Configure Redis
- Configure Full Page Cache
- Configure Sessions
- Configure OpenSearch
- Compile Dependency Injection
- Deploy Static Content
- Flush Cache
- Reindex Catalog
- Configure Cron Jobs
- Verify Store Performance

---

# Step 1 — Verify Current Deployment Mode

```bash
docker exec -it magento-php php bin/magento deploy:mode:show
```

Expected

```
Current application mode:

production
```

If not in production mode:

```bash
docker exec -it magento-php php bin/magento deploy:mode:set production
```

---

# Step 2 — Verify Redis Configuration

Check Magento Redis configuration.

```bash
docker exec -it magento-php php bin/magento config:show cache/frontend/default/backend
```

Expected

```
Cm_Cache_Backend_Redis
```

---

Check Redis server

```bash
docker exec -it magento-redis redis-cli ping
```

Expected

```
PONG
```

---

# Step 3 — Verify Full Page Cache

Check Full Page Cache configuration.

```bash
docker exec -it magento-php php bin/magento cache:status
```

Expected

```
full_page : 1
```

---

Verify cache backend

```bash
docker exec -it magento-php php bin/magento config:show system/full_page_cache/caching_application
```

Expected

```
2
```

Redis should be configured as the Full Page Cache backend.

---

# Step 4 — Verify Session Storage

Verify

```bash
docker exec -it magento-php php -i | grep session.save_handler
```

Expected

```
redis
```

---

# Step 5 — Verify OpenSearch

Check OpenSearch status

```bash
curl http://localhost:9200
```

Expected

```
cluster_name

status

version
```

---

Verify Magento Search Engine

```bash
docker exec -it magento-php php bin/magento config:show catalog/search/engine
```

Expected

```
opensearch
```

---

# Step 6 — Check OpenSearch Health

```bash
curl http://localhost:9200/_cluster/health?pretty
```

Expected

```
green
```

or

```
yellow
```

---

# Step 7 — Dependency Injection Compilation

Compile Magento.

```bash
docker exec -it magento-php php bin/magento setup:di:compile
```

Expected

```
Compilation was started.

Compilation complete.
```

---

# Step 8 — Deploy Static Content

Deploy production assets.

```bash
docker exec -it magento-php php bin/magento setup:static-content:deploy -f en_US
```

Expected

```
Deployment completed.
```

---

# Step 9 — Flush Cache

```bash
docker exec -it magento-php php bin/magento cache:flush
```

---

# Step 10 — Reindex

```bash
docker exec -it magento-php php bin/magento indexer:reindex
```

Expected

```
All indexers rebuilt successfully.
```

---

# Step 11 — Verify Indexers

```bash
docker exec -it magento-php php bin/magento indexer:status
```

Expected

```
Ready
```

---

# Step 12 — Configure Cron Jobs

Magento relies heavily on scheduled tasks.

Edit crontab

```bash
crontab -e
```

Add

```cron
* * * * * docker exec magento-php php /var/www/html/bin/magento cron:run >> /var/log/magento-cron.log
```

Save

Verify

```bash
crontab -l
```

---

# Step 13 — Run Cron Manually

```bash
docker exec -it magento-php php bin/magento cron:run
```

Repeat three times.

---

# Step 14 — Verify Cache Status

```bash
docker exec -it magento-php php bin/magento cache:status
```

Expected

Every cache should be enabled.

---

# Step 15 — Verify Static Assets

```bash
curl -I https://yourdomain.com/static/frontend/Magento/luma/en_US/css/styles-m.css
```

Expected

```
HTTP/2 200
```

---

# Step 16 — Verify Homepage

```bash
curl -I https://yourdomain.com
```

Expected

```
HTTP/2 200
```

---

# Step 17 — Verify CSS

Open browser

Developer Tools

Network

Refresh

Verify

```
styles-m.css

styles-l.css

HTTP 200
```

---

# Step 18 — Verify Store Performance

Use

```
GTMetrix

PageSpeed Insights

Lighthouse
```

Verify

- Static Assets
- Compression
- HTTPS
- HTTP/2
- Cache Headers

---

# Useful Magento Commands

Flush Cache

```bash
php bin/magento cache:flush
```

Enable Maintenance

```bash
php bin/magento maintenance:enable
```

Disable Maintenance

```bash
php bin/magento maintenance:disable
```

Reindex

```bash
php bin/magento indexer:reindex
```

Compile

```bash
php bin/magento setup:di:compile
```

Deploy Static Content

```bash
php bin/magento setup:static-content:deploy -f
```

Cron

```bash
php bin/magento cron:run
```

---

# Common Issues

## CSS Missing

Run

```bash
php bin/magento setup:static-content:deploy -f
```

---

## Cache Not Working

Restart Redis

```bash
docker restart magento-redis
```

---

## Search Not Working

Restart OpenSearch

```bash
docker restart magento-opensearch
```

---

## Slow Store

Verify

- Production Mode
- Redis
- Static Content
- OPcache
- OpenSearch

---

## Indexer Invalid

Run

```bash
php bin/magento indexer:reindex
```

---

# Best Practices

✔ Production Mode

✔ Redis Cache

✔ Redis Sessions

✔ Redis Full Page Cache

✔ OpenSearch

✔ Dependency Injection Compilation

✔ Static Content Deployment

✔ HTTPS

✔ HTTP/2

✔ Cron Jobs

✔ Docker Compose

✔ Optimized PHP Settings

---

# Validation Checklist

| Validation | Status |
|------------|--------|
| Production Mode Enabled | ✅ |
| Redis Running | ✅ |
| Full Page Cache Enabled | ✅ |
| Sessions Stored in Redis | ✅ |
| OpenSearch Running | ✅ |
| Search Working | ✅ |
| Static Content Deployed | ✅ |
| DI Compilation Complete | ✅ |
| Indexers Ready | ✅ |
| Cron Configured | ✅ |
| Store Accessible | ✅ |
| Admin Accessible | ✅ |

---

# Screenshots

Capture the following screenshots.

---

## Production Mode

```
screenshots/production-mode.png
```

```markdown
![Production Mode](../screenshots/production-mode.png)
```

---

## Redis Ping

```
screenshots/redis-ping.png
```

```markdown
![Redis](../screenshots/redis-ping.png)
```

---

## OpenSearch Health

```
screenshots/opensearch-health.png
```

```markdown
![OpenSearch](../screenshots/opensearch-health.png)
```

---

## DI Compilation

```
screenshots/di-compile.png
```

```markdown
![DI Compile](../screenshots/di-compile.png)
```

---

## Static Content Deployment

```
screenshots/static-content.png
```

```markdown
![Static Content](../screenshots/static-content.png)
```

---

## Cache Status

```
screenshots/cache-status.png
```

```markdown
![Cache Status](../screenshots/cache-status.png)
```

---

## Indexer Status

```
screenshots/indexer-status.png
```

```markdown
![Indexer Status](../screenshots/indexer-status.png)
```

---

## Cron Configuration

```
screenshots/cron-job.png
```

```markdown
![Cron](../screenshots/cron-job.png)
```

---

## Magento Homepage

```
screenshots/final-homepage.png
```

```markdown
![Homepage](../screenshots/final-homepage.png)
```

---

## Magento Admin Dashboard

```
screenshots/admin-dashboard.png
```

```markdown
![Admin Dashboard](../screenshots/admin-dashboard.png)
```

---

# Skills Demonstrated

By completing this phase, you have demonstrated the ability to:

- Configure Magento for Production Mode
- Integrate Redis for cache, sessions, and full-page caching
- Configure and validate OpenSearch
- Compile Dependency Injection
- Deploy static assets for production
- Configure and verify Magento cron jobs
- Reindex Magento data
- Validate application performance and health
- Troubleshoot production deployment issues
- Apply production best practices for Magento on Docker

---

# Final Outcome

At the end of this phase, the Magento 2 application is fully optimized for a production environment.

The deployment now includes:

- Production Mode enabled
- Redis configured for cache, sessions, and full-page cache
- OpenSearch integrated and operational
- Dependency Injection compiled
- Static content deployed
- Indexers rebuilt and healthy
- Cron jobs configured and running
- HTTPS-enabled storefront with optimized performance

This marks the completion of a production-ready Magento stack suitable for real-world deployments.

---

# 🚀 Next Phase

**Phase 11 — Securing Magento with Domain, SSL (Let's Encrypt), DNS Configuration & Final Production Validation**

In the next phase, we will cover:

- Purchasing and configuring a custom domain
- Configuring DNS records (GoDaddy → AWS EC2)
- Installing SSL certificates using Let's Encrypt
- Configuring HTTPS redirection
- Updating Magento Base URLs
- Verifying SSL and HTTP/2
- Performing final end-to-end production validation
- Preparing the application for public access
...
