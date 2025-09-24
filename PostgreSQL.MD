### PostgreSQL 13 Installation & Configuration Guide (RHEL 9)

Developer has chosen the database PostgreSQL. Hence, we are trying to install it up and configure it.

Versions of the DB Software you will get context from the developer, Meaning we need to check with developer.

---

### PostgreSQL 13 Installation & Setup Guide on RHEL 9

This guide helps you install and configure PostgreSQL 13 on a RHEL-based system (RHEL 9, CentOS Stream 9, Rocky Linux 9, AlmaLinux 9). And you can change this commands as per your distribution or Operating System.

---

## Step-by-Step Installation

### Add the PostgreSQL Official YUM Repository.
This command adds the official PostgreSQL YUM repository to your system so you can install newer versions.
```
sudo dnf install -y https://download.postgresql.org/pub/repos/yum/reporpms/EL-9-x86_64/pgdg-redhat-repo-latest.noarch.rpm
```

---

### Disable the default PostgreSQL Module  
It disables the default system PostgreSQL module that might conflict with version 13.
```
sudo dnf -qy module disable postgresql
```
---

### Install PostgreSQL 13 Server Package  
It installs PostgreSQL 13 server and all necessary binaries.
```
sudo dnf install -y postgresql13-server
```
---

### Initialize the PostgreSQL Database  
It creates the data directory and necessary files.
```
sudo /usr/pgsql-13/bin/postgresql-13-setup initdb
```
---

### Enable PostgreSQL  
This ensures that PostgreSQL starts automatically after system reboot.
```
sudo systemctl enable postgresql-13
```
---

### Start PostgreSQL Service  
Starts the PostgreSQL server immediately.
```
sudo systemctl start postgresql-13
```
---

### Verify the Installation of PostgreSQL version
```
psql --version
```
---

## Enable Remote Access

### Edit postgresql.conf to listen on all IPs
```
sudo vi /var/lib/pgsql/13/data/postgresql.conf
```
Find:

`#listen_addresses = 'localhost'`

Change to:
```
listen_addresses = '*'
```
---

### Update pg_hba.conf to allow remote connections  
**Note**: 0.0.0.0/0 in place of add your subnet ID to tighten the security
```
sudo vi /var/lib/pgsql/13/data/pg_hba.conf
```
Add at the end:
```
host    all             all             0.0.0.0/0               md5
```
---

### Restart the PostgreSQL Server
```
sudo systemctl restart postgresql-13
```
---

### Connect to PostgreSQL Server as the Superuser  
Switches to the postgres system user:
```
sudo -i -u postgres
```
Opens the PostgreSQL command-line interface as the default superuser:
```
psql
```
---

## Creating the user with password, database and provide privileges

---

### Create a New User  
Creates a new PostgreSQL user named expense and assigns a secure password.  
(Use a strong one with letters, numbers, and symbols.)
```
CREATE USER expense WITH PASSWORD 'Admin@123';
```
---

### Create a New Database  
Creates a database called expense_db owned by the expense user.  
**Security tip**: Each app should have its own database and dedicated user.
```
CREATE DATABASE expense_db OWNER expense;
```
---

### Allow the User to Connect to the Database  
Grants permission to establish a connection to the database.
```
GRANT CONNECT ON DATABASE expense_db TO expense;
```
---

### Grant Access to the Schema  
Allows the user to access the schema structure.  
Without this, the user cannot interact with objects inside the schema like tables. (default: public)
```
GRANT USAGE ON SCHEMA public TO expense;
```
---

### Grant CRUD Privileges on Existing Tables  
Grants the user permission to read (SELECT), insert, update, and delete data.  
These are standard application-level permissions.
```
GRANT SELECT, INSERT, UPDATE, DELETE ON ALL TABLES IN SCHEMA public TO expense;
```
---

### Grant Same Privileges on Future Tables  
Ensures that any new tables created in the future also grant the same CRUD privileges to expense user.  
Without this, future tables will not be accessible by default.
```
ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT SELECT, INSERT, UPDATE, DELETE ON TABLES TO expense;
```
---

## PostgreSQL Usage Reference

### To list the databases in the server:

```
\l
```

### To connect the expense_db database:

```
\c expense_db
```

### To list the tables in the expense_db database:

```
\dt
```
---

### You can see entries of a table using:
**Note**: After downloading code and run the project table will create automatically then you can see tables in database server
```
SELECT * FROM expense_db;
```
---
