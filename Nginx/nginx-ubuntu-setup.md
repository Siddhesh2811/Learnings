# NGINX Installation & Configuration Guide

This document provides step-by-step instructions to install, configure, and manage NGINX on Ubuntu (Jammy) with custom logging, user setup, and log rotation.

---

## 1. Install NGINX Key and Add Repository

```bash
wget https://nginx.org/keys/nginx_signing.key
apt-key add nginx_signing.key
```

Edit the `sources.list` file:

```bash
vi /etc/apt/sources.list
```

Add the following lines:

```
deb https://nginx.org/packages/ubuntu/ jammy nginx
deb-src https://nginx.org/packages/ubuntu/ jammy nginx
```

Remove old nginx packages:

```bash
apt-get remove nginx-common
```

Update repositories:

```bash
apt-get update
```

---

## 2. Check Available NGINX Versions

```bash
apt list nginx -a
```

---

## 3. Install Specific Version of NGINX

Example: Install version `1.24.0*`

```bash
apt-get install nginx=1.24.0*
```

---

## 4. Enable and Start NGINX

Enable auto-start:

```bash
systemctl enable nginx
```

Start service:

```bash
systemctl start nginx
```

Check status:

```bash
systemctl status nginx
```

---

## 5. Configure NGINX

Rename existing config and create a new `nginx.conf`:

```bash
mv /etc/nginx/nginx.conf /etc/nginx/nginx.conf.bak
vi /etc/nginx/nginx.conf
```

Example configuration:

```nginx
user appadm;
worker_processes auto;
pid /run/nginx.pid;

events {
    worker_connections 1024;
    multi_accept on;
}

http {
    sendfile on;
    tcp_nopush on;
    tcp_nodelay on;
    keepalive_timeout 65;
    client_max_body_size 25M;
    types_hash_max_size 2048;
    server_tokens off;
    include /etc/nginx/mime.types;
    default_type application/octet-stream;

    log_format main '[$time_local] - '
        '"Remote_Address=$remote_addr" - '
        '"Remote_user=$remote_user " - '
        '"Request=$request" - '
        '"Status=$status" - '
        '"Body_bytes_sent=$body_bytes_sent" - '
        '"HTTP_Referer=$http_referer" - '
        '"x-forwarded_for=$http_x_forwarded_for" - '
        '"Request_time=$request_time" - '
        '"Upstream_response_time=$upstream_response_time" - '
        '"Upstream_header_time=$upstream_header_time" - '
        '"Upstream_connect_time=$upstream_connect_time" - '
        '"Pipe=$pipe" - '
        '"connection=$connection" - '
        '"connection_requests=$connection_requests" - '
        '"Upstream_Address=$upstream_addr" - '
        '"Upstream_status=$upstream_status" - '
        '"scheme=$scheme" - '
        '"connection_Active=$connections_active" - '
        '"connections_reading=$connections_reading" - '
        '"connections_writing=$connections_writing" - '
        '"connections_waiting=$connections_waiting" - '
        '"arg_url=$arg_url"';

    # Logging paths
    access_log /app/data/log/nginx/access.log main;   # Non-Prod
    error_log /app/data/log/nginx/error.log;          # Non-Prod

    access_log /app/log/nginx/access.log main;        # Prod
    error_log /app/log/nginx/error.log;               # Prod

    gzip on;
    include /etc/nginx/conf.d/*.conf;
    include mime.types;
    proxy_read_timeout 100;

    gzip_comp_level 1;
    gzip_min_length 1000;
    gzip_proxied expired no-cache no-store private auth;
    gzip_types text/plain application/x-javascript text/xml text/css application/xml;
}
```

---

## 6. Create Log Folders

For Non-Prod:

```bash
mkdir -p /app/data/log/nginx
```

For Prod:

```bash
mkdir -p /app/log/nginx
```

---

## 7. Configure Sudo for `appadm` User

Change permissions of `sudoers`:

```bash
chmod 770 /etc/sudoers
vi /etc/sudoers
```

Add the following:

```
appadm ALL= NOPASSWD: /usr/bin/systemctl start nginx.service
appadm ALL= NOPASSWD: /usr/bin/systemctl stop nginx.service
appadm ALL= NOPASSWD: /usr/bin/systemctl restart nginx.service
appadm ALL= NOPASSWD: /usr/bin/systemctl status nginx.service
appadm ALL= NOPASSWD: /usr/bin/systemctl start nginx
appadm ALL= NOPASSWD: /usr/bin/systemctl stop nginx
appadm ALL= NOPASSWD: /usr/bin/systemctl restart nginx
appadm ALL= NOPASSWD: /usr/bin/systemctl status nginx
```

Revert permissions:

```bash
chmod 440 /etc/sudoers
```

Change ownership of `/app` folder:

```bash
chown -R appadm:appadm /app
```

---

## 8. Configure Log Rotation

Delete existing nginx logrotate config and create a new one:

```bash
vi /etc/logrotate.d/nginx
```

Add:

```
/app/log/nginx/*.log {      # Prod
/app/data/log/nginx/*.log { # Non-Prod
    daily
    missingok
    rotate 30
    notifempty
    create 740 appadm appadm
    dateext
    sharedscripts
    postrotate
        if [ -f /run/nginx.pid ]; then
            kill -USR1 `cat /run/nginx.pid`
        fi
    endscript
}
```

---

## 9. Restart NGINX

```bash
systemctl restart nginx
systemctl status nginx
```

Check running processes:

```bash
ps -ef | grep -i nginx
```

---

✅ Now, NGINX is installed, configured, and running with **appadm** user, with proper logging and log rotation.
