# Pre-requisites

```bash
sudo apt update
sudo apt install postgresql postgresql-contrib -y
# Optional, if you need PostGIS
sudo apt install postgis postgresql-<version>-postgis-3 -y
```

---

## Configure PostgreSQL for replication

## Step 3.1: Configure PostgreSQL for replication (vm1)

### Edit `postgresql.conf`

```bash
sudo nano /etc/postgresql/<version>/main/postgresql.conf
```

```bash
listen_addresses = '*'
wal_level = replica
max_wal_senders = 10
wal_keep_size = 256MB
```

### Edit `pg_hba.conf`

```bash
sudo nano /etc/postgresql/16/main/pg_hba.conf
```

**This allows VM2 to connect for replication.**

```bash

# You only added:
host replication postgres 192.168.56.102/32 md5

# Add this too (recommended):
host all postgres 192.168.56.102/32 md5

# So final section should be:
host replication postgres 192.168.56.102/32 md5
host all         postgres 192.168.56.102/32 md5

sudo systemctl restart postgresql

```

## Set Password for postgres User BOTH(vm1, vm2)

```bash
ALTER USER postgres WITH PASSWORD 'postgres';
```

## Configure SLAVE (VM2)

```bash
# Stop PostgreSQL
sudo systemctl stop postgresql

# Use safer and version-specific:
sudo rm -rf /var/lib/postgresql/16/main
sudo mkdir -p /var/lib/postgresql/16/main
sudo chown postgres:postgres /var/lib/postgresql/16/main
sudo chmod 700 /var/lib/postgresql/16/main

# Take Base Backup from Master
sudo -u postgres pg_basebackup \
-h 192.168.56.101 \
-D /var/lib/postgresql/14/main \
-U postgres \
-P -v -R

# It is asking for the MASTER database password, not the slave password.
When prompted, enter the MASTER PostgreSQL password
(for example: postgres)

```

## ✅ Cleaned & Production-Ready Version

### Edit postgresql.conf (VM1)

```bash
sudo nano /etc/postgresql/16/main/postgresql.conf

listen_addresses = '*'
wal_level = replica
max_wal_senders = 10
wal_keep_size = 256MB

sudo systemctl restart postgresql
```

### Edit pg_hba.conf (VM1)

```bash
sudo nano /etc/postgresql/16/main/pg_hba.conf

host replication postgres 192.168.56.102/32 md5
host all         postgres 192.168.56.102/32 md5

sudo systemctl restart postgresql
```

### Configure SLAVE (VM2)

```bash
sudo systemctl stop postgresql

sudo rm -rf /var/lib/postgresql/16/main
sudo mkdir -p /var/lib/postgresql/16/main
sudo chown postgres:postgres /var/lib/postgresql/16/main
sudo chmod 700 /var/lib/postgresql/16/main

sudo -u postgres pg_basebackup \
-h 192.168.56.101 \
-D /var/lib/postgresql/16/main \
-U postgres \
-P -v -R
```