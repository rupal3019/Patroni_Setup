# Patroni Setup

This repository contains the setup for Patroni, a PostgreSQL high availability solution.

## Table of Contents

- [Overview](#overview)
- [Requirements](#requirements)
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
- [Backup](#backup)
- [Maintenance](#maintenance)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)

## Overview

Patroni is an HA (high availability) solution for PostgreSQL, providing automatic failover and recovery. This setup includes configurations for running Patroni with PostgreSQL, ensuring high availability and reliability.

## Requirements
- Pgbouncer
- PostgreSQL 12
- Patroni
- etcd (for distributed consensus)
- Python 3.6+

<!-- ## Installation

1. **Clone the Repository**
   ```bash
   git clone https://github.com/your-username/patroni-setup.git
   cd patroni-setup -->

## Installation

### Perform these steps on all three nodes

1. **Update and Upgrade Packages**
   ```bash
    sudo apt-get update
    sudo apt-get upgrade -y 

2. **Install PostgreSQL**
    ```bash
    sudo apt install postgresql postgresql-contrib -y  
    dpkg --status postgresql
    sudo systemctl stop postgresql

3. **Install Python and Dependencies**
    ```bash
    sudo apt install python3-pip python3-dev libpq-dev -y  
    sudo pip install patroni==2.1.4
    sudo pip install python-etcd
    sudo pip install psycopg2
    sudo pip install --prefix /usr/local patroni    
    sudo pip install --prefix /usr/local psycopg2-binary    
    sudo pip install --prefix /usr/local psycopg2    
    sudo pip install --prefix /usr/local python-etcd  
    sudo ln -s /usr/lib/postgresql/12/bin/* /usr/sbin/

4. **Create Patroni Configuration File**
    ```bash
    sudo nano /etc/patroni.yml  
    sudo chown -R postgres:postgres /etc/patroni.yml

5. **Create Directories for Patroni**
    ```bash
    sudo mkdir -p /mnt/patroni ~/archivedir
    sudo chown postgres:postgres /mnt/patroni ~/archivedir 
    sudo chmod 775 ~/archivedir
    sudo chmod 700 /mnt/patroni

6. **Create Patroni Service File**
    ```bash
    sudo nano /etc/systemd/system/patroni.service

7. **Configure PostgreSQL**
    ```bash
    sudo nano /etc/postgresql/12/main/postgresql.conf  
    sudo nano /etc/postgresql/12/main/pg_hba.conf
    sudo systemctl restart postgresql.service

8. **Configure PostgreSQL Database Users and Roles**
    ```bash
    sudo -u postgres psql
    ```
    In PostgreSQL shell:
    ```bash
    -- change the password of postgres and  create user
    ALTER USER postgres PASSWORD '874cda45d25772ae19bbbeee';
    CREATE USER test_user WITH PASSWORD '655978f955aed22449198bed';
    SELECT * FROM pg_authid;

9. **Install PgBouncer and configure it**
    ```bash
    sudo apt-get install pgbouncer -y
    ```
    
    Create userlist.txt:
    
    ```bash
    sudo nano /etc/pgbouncer/userlist.txt
    ```

    Add the user details:

    ```bash
    "postgres" "md55e713b91809cb3340b6ad395a309d06e"
    "test_user" "md59c616e3947d862a"
    ```

    Configure PgBouncer:

    ```bash
    sudo nano /etc/pgbouncer/pgbouncer.ini
    ```

10. **Prepare PostgreSQL Data Directory**
    ```bash
    sudo systemctl stop postgresql.service      
    sudo mv /var/lib/postgresql/12/main /var/lib/postgresql/12/main_old

11. **Reload Daemon and Start Services**
    ```bash
    sudo systemctl daemon-reload
    sudo systemctl restart patroni.service
    sudo systemctl status patroni.service
    sudo systemctl enable patroni.service
    sudo systemctl disable postgresql.service  
    sudo systemctl enable pgbouncer.service
    sudo systemctl status pgbouncer.service
    sudo systemctl status patroni.service

12. Do not Start PostgreSQL Directly

13. **Verify Setup**
    ```bash
    patronictl -c /etc/patroni.yml list
    ```







