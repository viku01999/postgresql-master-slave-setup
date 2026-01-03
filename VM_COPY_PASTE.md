# 🖥️ VirtualBox Clipboard & Drag-and-Drop Setup Guide

This guide walks you through enabling **Clipboard** and **Drag-and-Drop** features in VirtualBox, including installing **Guest Additions** for full functionality.

---

## ✅ Step 1: Enable Clipboard & Drag-and-Drop (Host Side)

> Make sure the VM is **shut down completely** (do not pause).

1. Open **VirtualBox Manager**.
2. Select your VM → **Settings**.
3. Go to **General → Advanced**.
4. Configure the following options:

| Feature | Setting |
|---------|---------|
| Shared Clipboard | Bidirectional |
| Drag’n’Drop | Bidirectional |

5. Click **OK**.

6. Start the VM again.

> ⚠️ Note: This alone is **not enough**. You need **Guest Additions** installed on the VM for these features to work.

---

## ✅ Step 2: Install Guest Additions (MOST IMPORTANT)

### 2.1 Insert Guest Additions CD

Inside the running VM:

- From the VM window menu:  
  `Devices → Insert Guest Additions CD Image`
- If prompted → **Click Run**  
- If not prompted → run manually (see below).

---

### 2.2 Install Required Packages inside VM

Open a terminal in the VM and run:

```bash
sudo apt update
sudo apt install -y build-essential dkms linux-headers-$(uname -r)
```

### 2.3 Install Required Packages inside VM

```bash
sudo mkdir -p /media/cdrom
sudo mount /dev/cdrom /media/cdrom
sudo sh /media/cdrom/VBoxLinuxAdditions.run
```

### 2.4 Reboot the VM

```bash
sudo reboot
```

### 2.5 Verify Clipboard Service Is Running

```bash
# After reboot, run inside VM:
ps aux | grep VBoxClient
```
