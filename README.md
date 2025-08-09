# Authorative-DNS-Server-Full-Setup-Bind9
---


https://drive.google.com/file/d/10R23XYZrVht18LCzwLodNarcwqZIB0d0/view?usp=sharing

```markdown
# 🧭 holalho.online DNS Server Setup

This project documents the full setup of an **authoritative DNS server** for the domain `holalho.online` using **BIND9** on an **AWS EC2 Ubuntu instance**. It includes zone configuration, firewall setup, domain registrar integration, and troubleshooting.

---

## 📦 Project Overview

- **Domain**: `holalho.online`
- **Nameservers**: `ns1.holalho.online`, `ns2.holalho.online`
- **Server IP**: `54.90.140.50`
- **Platform**: Ubuntu EC2 (AWS)
- **DNS Software**: BIND9

---

## 🛠️ Setup Steps

### 1. 🔐 EC2 Security Group Configuration

Allow DNS traffic:

```text
Inbound Rules:
- UDP Port 53 from 0.0.0.0/0
- TCP Port 53 from 0.0.0.0/0
```

### 2. 🌐 Domain Configuration (Namecheap)

Update DNS records:

| Type | Host | Value               | TTL     |
|------|------|---------------------|---------|
| A    | @    | 54.90.140.50        | 30 min  |
| A    | www  | 54.90.140.50        | 30 min  |
| NS   | @    | ns1.holalho.online  | 30 min  |
| NS   | @    | ns2.holalho.online  | 30 min  |

### 3. ⚙️ BIND9 Configuration

#### `named.conf.local`

```bash
zone "holalho.online" {
    type master;
    file "/etc/bind/zones/db.holalho.online";
};
```

#### Zone File: `/etc/bind/zones/db.holalho.online`

```bash
$TTL 86400
@   IN  SOA ns1.holalho.online. admin.holalho.online. (
        2025080901 ; Serial
        3600       ; Refresh
        1800       ; Retry
        604800     ; Expire
        86400 )    ; Minimum TTL

    IN  NS  ns1.holalho.online.
    IN  NS  ns2.holalho.online.

ns1 IN  A   54.90.140.50
ns2 IN  A   54.90.140.50
@   IN  A   54.90.140.50
www IN  A   54.90.140.50
```

### 4. 🔄 Service Management

```bash
sudo systemctl start bind9
sudo systemctl enable bind9
```

### 5. ✅ Validation

```bash
named-checkconf
named-checkzone holalho.online /etc/bind/zones/db.holalho.online
dig @54.90.140.50 holalho.online SOA
```

---

## 📹 Video Tutorials

These videos provide visual guidance and deeper understanding of DNS server setup:

1. [Linux DNS Server Configuration for Beginners](https://www.youtube.com/watch?v=I8lawEbZKxA)  
   Explains how to install and configure BIND9 on Ubuntu, ideal for beginners.

2. [You want a real Name Server at home? // DNS](https://www.youtube.com/watch?v=syzwLwE3Xq4)  
   Shows how to deploy BIND9 in a home lab and configure zones.

3. [How to Configure DNS Server Correctly on Windows Server](https://www.youtube.com/watch?v=6l2T7-4dJis)  
   Demonstrates zone setup and record management in a Windows environment.

4. [3.1 Implementing DNS on Windows Server 2016 (Step by Step)](https://www.youtube.com/watch?v=P6KEXb1pIFg)  
   Covers SOA records, primary/secondary zones, and DNS replication.

5. [Windows Server 2022 Setting Up DNS](https://www.youtube.com/watch?v=n1bojk5amj8&pp=ygUOI3NlcnZlcjIwMjJkbnM%3D)  
   Walks through zone creation and troubleshooting DNS issues.

6. [How to Install and Configure DNS on Windows Server 2022](https://www.youtube.com/watch?v=OtdOEiTozUE)  
   A complete guide to installing DNS roles and configuring forwarders.

---

## 🧪 Troubleshooting

### ❌ DiG Timeout Error

```text
;; communications error to 54.90.140.50#53: timed out
```

**Fixes:**
- Opened port 53 in EC2 security group
- Verified BIND9 is running
- Corrected DNS records on Namecheap
- Confirmed zone file and SOA record

---

## 📚 References

- [Ubuntu DNS Server Setup Guide](https://documentation.ubuntu.com/server/how-to/networking/install-dns/)  
- [Microsoft DNS Server Quickstart](https://learn.microsoft.com/en-us/windows-server/networking/dns/quickstart-install-configure-dns-server)  
- [DNS Configuration Explained – phoenixNAP](https://phoenixnap.com/kb/dns-configuration)

---

## 📄 License

MIT License

---

## 🙋‍♂️ Author

**Abdullrahman Eissa**  
Feel free to reach out for questions or improvements!
```

---
