
# 🚀 NGINX Installation and Configuration Guide

---

## 🔹 1️⃣ Install YUM Utilities
```bash
yum install yum-utils
```
👉 Installs helper tools like `yum-config-manager`.

---

## 🔹 2️⃣ Create NGINX YUM Repository
```bash
vi /etc/yum.repos.d/nginx.repo
```
👉 Create repo config file.

---

## 🔹 3️⃣ Add Repository Content
```ini
[nginx-stable]
name=nginx stable repo
baseurl=http://nginx.org/packages/rhel/8/$basearch/
gpgcheck=1
enabled=1
gpgkey=https://nginx.org/keys/nginx_signing.key
module_hotfixes=true

[nginx-mainline]
name=nginx mainline repo
baseurl=http://nginx.org/packages/rhel/8/$basearch/
gpgcheck=1
enabled=0
gpgkey=https://nginx.org/keys/nginx_signing.key
module_hotfixes=true
```
👉 Sets up NGINX stable (enabled) and mainline (disabled) repos.

---

## 🔹 4️⃣ Enable Stable NGINX Repository
```bash
yum-config-manager --enable nginx-stable
```
👉 Enables `nginx-stable` repo for package installation.

---

## 🔹 5️⃣ Update Packages (excluding OS release)
```bash
yum update -x redhat-release*
```
👉 Updates all except OS version package.

---

## 🔹 6️⃣ Install NGINX
```bash
yum install nginx-<version>
```
👉 Installs NGINX.

---

## 🔹 7️⃣ Verify NGINX Version
```bash
nginx -v
```
👉 Displays installed version.

---

## 🔹 8️⃣ Configure NGINX
### Backup Default Config and Create New
```bash
cd /etc/nginx/
mv nginx.conf nginx.conf_orig
vi nginx.conf
```
👉 Backs up default and creates new config.

### New nginx.conf Content
<details>
<summary>Click to expand</summary>

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
    '"Upstream_response_time="$upstream_response_time" - '
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

    access_log /app/log/nginx/access.log main;   # For Prod
    error_log /app/log/nginx/error.log;          # For Prod

    access_log /app/data/log/nginx/access.log main; # For Non-Prod
    error_log /app/data/log/nginx/error.log;        # For Non-Prod

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
</details>

---

## 🔹 9️⃣ Create Log Directories
```bash
# Non-Prod
mkdir -p /app/data/log/nginx

# Prod
mkdir -p /app/log/nginx
```
👉 Prepares NGINX log directories.

---

## 🔹 🔑 Sudo Configuration for appadm
### a️. Change sudoers permission temporarily
```bash
chmod 770 /etc/sudoers
```
👉 Temporarily allows editing.

### b️. Edit sudoers to allow NGINX controls
```bash
vi /etc/sudoers
```
➡ Add:
```
appadm ALL=NOPASSWD: /usr/bin/systemctl start nginx.service
appadm ALL=NOPASSWD: /usr/bin/systemctl stop nginx.service
appadm ALL=NOPASSWD: /usr/bin/systemctl restart nginx.service
appadm ALL=NOPASSWD: /usr/bin/systemctl status nginx.service
appadm ALL=NOPASSWD: /usr/bin/systemctl reload nginx.service
appadm ALL=NOPASSWD: /usr/bin/systemctl start nginx
appadm ALL=NOPASSWD: /usr/bin/systemctl stop nginx
appadm ALL=NOPASSWD: /usr/bin/systemctl reload nginx
appadm ALL=NOPASSWD: /usr/bin/systemctl restart nginx
appadm ALL=NOPASSWD: /usr/bin/systemctl status nginx
```

### c. Revert sudoers permission
```bash
chmod 440 /etc/sudoers
```
👉 Locks down sudoers.

---

## 🔹 🔑 Set Ownership
```bash
chown -R appadm:appadm /app
```
👉 Assigns `/app` to appadm.

---

## 🔹 Logrotate Setup
```bash
vi /etc/logrotate.d/nginx
```
➡ Example content:
```
/app/log/nginx/*.log {      # For Prod
#/app/data/log/nginx/*.log { # For Non-Prod
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
👉 Rotates logs daily, keeps 30, signals NGINX after rotate.

---

## 🔹 Restart NGINX
```bash
systemctl restart nginx
systemctl status nginx
```
👉 Restarts and verifies NGINX.

---

## 🔹 Verify NGINX Process Ownership
```bash
ps -ef | grep -i nginx
```
👉 Confirms NGINX runs under `appadm` except master/root process.

---

✅ **NGINX setup complete**
