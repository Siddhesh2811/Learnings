# Neo4j Installation on Debian / Ubuntu (Java 21)

This guide provides step-by-step instructions to install and configure Neo4j on Debian-based Linux systems using OpenJDK 21.

## 1. Prerequisites
- Debian 11 / 12 or Ubuntu 20.04+
- Root or sudo access
- Internet access

## 2. Update System Packages
sudo apt update
sudo apt upgrade -y

## 3. Install OpenJDK 21
sudo apt install openjdk-21-jdk -y
java -version

## 4. Add Neo4j APT Repository
sudo mkdir -p /etc/apt/keyrings
wget -O - https://debian.neo4j.com/neotechnology.gpg.key | sudo gpg --dearmor -o /etc/apt/keyrings/neotechnology.gpg
sudo chmod a+r /etc/apt/keyrings/neotechnology.gpg
echo "deb [signed-by=/etc/apt/keyrings/neotechnology.gpg] https://debian.neo4j.com stable latest" | sudo tee /etc/apt/sources.list.d/neo4j.list
sudo apt update

## 5. Install Neo4j
sudo apt install neo4j -y

## 6. Enable and Start Neo4j
sudo systemctl enable neo4j
sudo systemctl start neo4j

## 7. Set Initial Password
sudo systemctl stop neo4j
sudo neo4j-admin dbms set-initial-password 'Password#123'
sudo systemctl start neo4j

## 8. Access Neo4j
http://<server-ip>:7474
