# 🖥️ VM Network Setup for PostgreSQL Master–Slave Replication

This guide explains how to set up **two Ubuntu latest VMs** on a single host using VirtualBox with proper network configuration for **Master–Slave (Primary–Replica) PostgreSQL replication**.

---

## 1️⃣ VM Requirements

Each VM should have the following:

| Resource   | Recommendation             |
|-----------|----------------------------|
| 🖥️ OS        | Ubuntu latest LTS           |
| ⚙️ CPU       | 1–2 cores                  |
| 🧠 RAM       | 1–2 GB                     |
| 💾 Disk      | 10–20 GB                   |
| 🌐 Network   | Adapter 1 → NAT, Adapter 2 → Host-only | (If adapter 2 disable or host only not available then follow Create Host-Only Network)
| 🗄️ PostgreSQL | Latest stable (14+)        |

**VMs will be assigned the following IPs - this can be different also:**

| VM   | Role   | Host-only IP       |
|------|--------|------------------|
| VM1  | Master | 192.168.30.101   |
| VM2  | Slave  | 192.168.30.102   |

---

## 2️⃣ 🔹 Create Host-Only Network (One-Time Setup)

1. Open **VirtualBox** → **File → Tools → Host Network Manager**  
2. Click **Create** → a network `vboxnet0` will be created  
3. Ensure settings:
4. Click **Apply** and **Close** Host Network Manager

---

## 3️⃣ Configure Network Adapters for VMs

### **VM1 (Master)**

- **Adapter 1**
  - Enable Network Adapter ✅
  - Attached to: **NAT**
- **Adapter 2**
  - Enable Network Adapter ✅
  - Attached to: **Host-only Adapter**
  - Name: `vboxnet0`
- Click **OK** to save

### **VM2 (Slave)**

- **Adapter 1**
  - Enable Network Adapter ✅
  - Attached to: **NAT**
- **Adapter 2**
  - Enable Network Adapter ✅
  - Attached to: **Host-only Adapter**
  - Name: `vboxnet0`
- Click **OK** to save

---

## 4️⃣ Assign Static IPs Inside Each VM

This guide shows how to set up a **Master–Slave network configuration** locally using **VirtualBox VMs** on a single host machine.  
The setup uses an **isolated host-only network (192.168.30.x)** to avoid conflicts with your office LAN and VPN.

- If VMs present into anotehr system then use adapter `bridge` not `host only`

---

## 📌 Environment Overview

| Machine | Role   | IP (Replication) | Notes |
|---------|--------|----------------|-------|
| Host    | Master | 192.168.30.100 | Office LAN IP (192.168.xx.x) unchanged |
| VM1     | Slave1 | 192.168.30.101 | Host-only network for replication |
| VM2     | Slave2 | 192.168.30.102 | Host-only network for replication |

## Host Machine Setup (Master)

1. Check existing network interfaces:

```bash
ifconfig
```

## Apply this on your host machine

**This can be different as per you system**
`vboxnet0`

```bash
sudo ip addr flush dev vboxnet0
sudo ip addr add 192.168.xx.100/24 dev vboxnet0
sudo ip link set vboxnet0 up
```

## Write configuration inside vm1

```bash
sudo nano /etc/netplan/00-installer-config.yaml
```

```yaml
network:
  version: 2
  ethernets:
    enp0s3:   # NAT adapter for Internet
      dhcp4: true
      dhcp6: true
    enp0s8:   # Host-only adapter for replication
      dhcp4: no
      addresses: [192.168.30.101/24]
```

## Write configuration inside vm2

```bash
sudo nano /etc/netplan/00-installer-config.yaml
```

```yaml
network:
  version: 2
  ethernets:
    enp0s3:   # NAT adapter for Internet
      dhcp4: true
      dhcp6: true
    enp0s8:   # Host-only adapter for replication
      dhcp4: no
      addresses: [192.168.30.102/24]
```

## Run this in both vms

```bash
   sudo systemctl restart systemd-networkd.service
   sudo netplan apply
   sudo reboot 
```

## Step 4 — Verify Full Network

### From **Host**

```bash
ping 192.168.30.101  # VM1
ping 192.168.30.102  # VM2
```

## file

![ifconfig_host_machine](/media/host_machine.png)

### From **vm1**

```bash
ping 192.168.30.100  # Host
ping 192.168.30.102  # VM2
```

![ifconfig_vm1_machine](/media/vm1_machine.png)

### From **vm2**

```bash
ping 192.168.30.100  # Host
ping 192.168.30.101  # VM1
```

![ifconfig_vm2_machine](/media/vm2_machine.png)
