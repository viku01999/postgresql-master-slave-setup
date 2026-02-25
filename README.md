# 🗄️ postgresql-master-slave-setup

- 🔵 PostgreSQL Master (Primary) Database  
- 🟢 PostgreSQL Slave (Read Replica) Database  
- 🚀 Spring Boot Application  
- 🔀 Automatic Read/Write Routing using AOP  
- 📡 Streaming Replication  

## 📄 Description

This repository contains documentation and instructions to set up a **PostgreSQL Master–Slave (Primary–Replica)** environment using multiple Ubuntu virtual machines. It is designed for **learning, testing, and small-scale development purposes**.

---

## 🧐 Overview

The goal of this project is to create a PostgreSQL replication setup where:

- **VM1 → Master (Primary):** Handles write operations and serves as the source of truth.  
- **VM2 → Slave (Replica):** Receives replicated data from the Master, providing redundancy and read scaling.  

This allows you to simulate real-world database replication, backups, and failover scenarios in a local environment.

---

## ✅ Prerequisites

Before setting up, ensure you have:

- **Virtualization software**: VirtualBox, VMware, or similar.
- **Two Ubuntu VMs** (Ubuntu 22.04 LTS recommended).
- **Basic knowledge of Linux commands and PostgreSQL.**
- **Network connectivity** between VMs (host-only or NAT network).
- **Sufficient system resources**:

| Resource  | Recommendation |
|-----------|----------------|
| CPU       | 1–2 cores      |
| RAM       | 1–2 GB         |
| Disk      | 10–20 GB       |

---

## 🖥️ VM Requirements

| VM      | Purpose  | Notes |
|---------|---------|-------|
| VM1     | Master  | Handles write operations and replication source |
| VM2     | Slave   | Replicates data from Master |

> Use static IPs for simplicity when configuring replication.

---

## ⚙️ System Requirements

### Master (Primary)

- Ubuntu Server 20.04 / 22.04 LTS  
- Stable kernel and updated packages  
- PostgreSQL installed  
- Sufficient disk space  
- Network reachable by Slave  

### Slave (Replica)

- Ubuntu Server 20.04 / 22.04 LTS  
- PostgreSQL installed (same major version as Master)  
- Network connectivity to Master  
- Sufficient disk space  

---

## 🌐 Network Configuration

Set up a host-only or NAT network with static IPs:

| VM        | Static IP Example |
|-----------|-----------------|
| Master    | 192.168.x.x      |
| Slave     | 192.168.x.x      |

> Replace example IPs with your VM’s actual IPs.

---

## 📝 Steps Overview

1. Install Ubuntu Server on each VM.  
2. Install PostgreSQL on both VMs.  
3. Configure Master for replication:
   - Enable WAL archiving  
   - Allow replication connections  
   - Create a replication user  
4. Configure Slave to replicate from Master:
   - Take a base backup from Master  
   - Configure `recovery.conf` or `standby.signal` for streaming replication  
5. Start PostgreSQL on Slave and verify replication.

---

## ⚠️ Notes

- This setup is intended for testing and learning.  
- Do not expose the database to public networks without security measures.  
- Ensure PostgreSQL versions match on Master and Slave for compatibility.

---

## 📚 References

- [PostgreSQL Streaming Replication Documentation](https://www.postgresql.org/docs/current/warm-standby.html)  
- [Ubuntu Server Installation](https://ubuntu.com/download/desktop/thank-you?version=24.04.3&architecture=amd64&lts=true)

## 💼 Connect & Project Repository

If you want to connect or see the Java practice project:

- 🔗 **LinkedIn Profile:** [Your LinkedIn](https://www.linkedin.com/posts/vikas-kumar-10b8822a9_suhora-activity-7158879542592364544-Rw02)  
- 🔗 **Java Repository:** [Master–Slave Spring Boot Project](https://github.com/viku01999/master-slave-setup)