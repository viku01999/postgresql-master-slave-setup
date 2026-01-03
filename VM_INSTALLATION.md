# 🖥️ VM Installation Guide

This document explains how to install **Ubuntu VMs** on a single system using **VirtualBox** and prepare them for PostgreSQL Master–Slave replication.

---

## 1. Install VirtualBox

Before running multiple VMs, you need VirtualBox installed. You can install it properly on Ubuntu as follows:

## 2. Download VM from

```bash
wget https://download.virtualbox.org/virtualbox/7.2.4/virtualbox-7.2_7.2.4-170995~Ubuntu~plucky_amd64.deb
```

## 3. Run these commands

```bash
sudo apt update
sudo apt upgrade -y
sudo apt install -y dkms build-essential linux-headers-$(uname -r) \
libqt6core6 libqt6dbus6 libqt6gui6 libqt6help6 libqt6printsupport6 \
libqt6statemachine6 libqt6widgets6 libqt6xml6 libtpms0 liblzf1
```

## 4. To install run

```bash
sudo dpkg -i virtualbox-7.2_7.2.4-170995~Ubuntu~plucky_amd64.deb
```

## 5. Check proper install or not

```cmd
vboxmanage --version
```

## 6. Running Multiple VMs on One System

```bash
virtualbox
```

## 7. Completely Removing VirtualBox

```bash
sudo apt remove --purge virtualbox-7.2 -y
sudo apt autoremove -y
sudo rm -rf ~/.config/VirtualBox
```
