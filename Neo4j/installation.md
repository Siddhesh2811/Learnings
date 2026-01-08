# 🚀 Neo4j Installation Guide (Debian / Ubuntu)

A **clean, step-by-step guide** to install **Neo4j (Community or Enterprise Edition)** on **Debian-based Linux systems** using **OpenJDK 21**, as required by Neo4j 5.x and later.

---

## 📋 Table of Contents
1. [Prerequisites](#-prerequisites)
2. [System Update](#-system-update)
3. [Install Java 21](#-install-java-21)
4. [Add Neo4j Repository](#-add-neo4j-repository)
5. [Install Neo4j](#-install-neo4j)
6. [Start & Enable Service](#-start--enable-service)
7. [Set Initial Password](#-set-initial-password)
8. [Access Neo4j](#-access-neo4j)
9. [Ports & Paths](#-ports--paths)
10. [Useful Commands](#-useful-commands)
11. [Production Notes](#-production-notes)
12. [Uninstall Neo4j](#-uninstall-neo4j)

---

## ✅ Prerequisites

- Debian 11 / 12 or Ubuntu 20.04+
- Root or sudo access
- Internet connectivity

---

## 🔄 System Update

```bash
sudo apt update
sudo apt upgrade -y
```

---

## ☕ Install Java 21

Neo4j requires **OpenJDK 21**.

```bash
sudo apt install openjdk-21-jdk -y
```

Verify:
```bash
java -version
```

Expected output:
```
openjdk version "21.x.x"
```

If multiple Java versions exist:
```bash
sudo update-java-alternatives --jre --set java-1.21.0-openjdk-amd64
```

---

## 📦 Add Neo4j Repository

### 1️⃣ Create Keyrings Directory
```bash
sudo mkdir -p /etc/apt/keyrings
```

### 2️⃣ Import GPG Key
```bash
wget -O - https://debian.neo4j.com/neotechnology.gpg.key \
 | sudo gpg --dearmor -o /etc/apt/keyrings/neotechnology.gpg
```

```bash
sudo chmod a+r /etc/apt/keyrings/neotechnology.gpg
```

### 3️⃣ Add Repository
```bash
echo "deb [signed-by=/etc/apt/keyrings/neotechnology.gpg] https://debian.neo4j.com stable latest" \
 | sudo tee /etc/apt/sources.list.d/neo4j.list > /dev/null
```

```bash
sudo apt update
```

---

## 🧠 Install Neo4j

### Community Edition
```bash
sudo apt install neo4j -y
```

### Enterprise Edition (Optional)
```bash
sudo apt install neo4j-enterprise -y
```

> ⚠️ Enterprise Edition requires license acceptance.

---

## ▶️ Start & Enable Service

```bash
sudo systemctl enable neo4j
sudo systemctl start neo4j
```

Check status:
```bash
sudo systemctl status neo4j
```

---

## 🔐 Set Initial Password

> ⚠️ Neo4j **must be stopped** before setting the initial password.

```bash
sudo systemctl stop neo4j
```

```bash
sudo neo4j-admin dbms set-initial-password 'Password#123'
```

```bash
sudo systemctl start neo4j
```

---

## 🌐 Access Neo4j

### Browser
```
http://<server-ip>:7474
```

### Login
```
Username: neo4j
Password: Password#123
```

### CLI (Optional)
```bash
cypher-shell -u neo4j -p 'Password#123'
```

---

## 🔌 Ports & Paths

### Default Ports
| Service | Port |
|-------|------|
| HTTP Browser | 7474 |
| HTTPS Browser | 7473 |
| Bolt | 7687 |

### Important Paths
| Purpose | Path |
|------|------|
| Config | /etc/neo4j/neo4j.conf |
| Data | /var/lib/neo4j |
| Logs | /var/log/neo4j |

---

## 🛠 Useful Commands

Restart:
```bash
sudo systemctl restart neo4j
```

Logs:
```bash
journalctl -u neo4j -f
```

---

## 🏭 Production Notes

✔ Change default password  
✔ Restrict ports using firewall  
✔ Tune heap & page cache in `neo4j.conf`  
✔ Use backups (Enterprise Edition)  
✔ Avoid public exposure without auth  

---

## 🧹 Uninstall Neo4j

```bash
sudo apt purge neo4j neo4j-enterprise -y
sudo rm -rf /var/lib/neo4j /var/log/neo4j /etc/neo4j
```

---

## 📚 References

- Neo4j Operations Manual (Debian Installation)
- Neo4j Java Compatibility Guide

---

✅ **Neo4j installation completed successfully.**
