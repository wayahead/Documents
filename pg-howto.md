# PostgreSQL Howto

## Install PostgreSQL 16 on Ubuntu 22.04

### Add APT Repository

```
$ sudo apt update
$ sudo apt install gnupg2 wget vim

$ sudo sh -c 'echo "deb https://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
$ curl -fsSL https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/postgresql.gpg
$ sudo apt update
```

### Install PostgreSQL

```
$ sudo apt install postgresql-16 postgresql-contrib-16
$ sudo systemctl start postgresql
$ sudo systemctl enable postgresql

$ psql --version
```

### Remove PostgreSQL

```
$ sudo dpkg -l | grep postgres
$ sudo apt-get --purge remove postgresql postgresql-16 postgresql-client-16 postgresql-client-common postgresql-commmon postgresql-contrib-16
```

## Configure PostgreSQL 16

### Allow Remote Connections (NOT RECOMMENDATION)

```
$ sudo vi /etc/postgresql/16/main/postgresql.conf
listen_addresses = 'localhost'
port = 6336
```

```
$ sudo vi /etc/postgresql/16/main/postgresql.conf
listen_addresses = '*'
port = 6336

$ sudo systemctl restart postgresql
$ sudo systemctl status postgresql
$ sudo ss -tulpn|grep postgres
$ ufw allow 6336/tcp
```

### Configure UFW Rules

```
$ sudo ufw allow proto tcp from 192.168.101/24 to any port 6336
```

### Use MD5 Password

```
$ sudo cp /etc/postgresql/16/main/pg_hba.conf /etc/postgresql/16/main/pg_hba.conf.orig
$ sudo sed -i '/^host/s/ident/md5/' /etc/postgresql/16/main/pg_hba.conf
$ sudo sed -i '/^local/s/peer/trust/' /etc/postgresql/16/main/pg_hba.conf
$ echo "host all all 127.0.0.1/32 md5" | sudo tee -a /etc/postgresql/16/main/pg_hba.conf
$ echo "host all all 0.0.0.0/0 md5" | sudo tee -a /etc/postgresql/16/main/pg_hba.conf

$ sudo systemctl restart postgresql
$ sudo systemctl status postgresql
$ sudo ss -tulpn|grep postgres
$ ufw allow 6336/tcp
```

### Set Password for `postgres`

```
$ sudo -u postgres psql
postgres=# ALTER USER postgres PASSWORD 'VeryStronGPassWord@1137';
postgres=# ALTER USER postgres PASSWORD 'WqA1yT2z+!';
postgres=# \q
```

## Admin Commands

### Create Password for `postgres`

```
$ sudo passwd postgres
WqA1yT2z+!
```

### Change Password for `postgres`

```
$ sudo -u postgres psql
postgres=# ALTER USER postgres PASSWORD 'VeryStronGPassWord@1137';
postgres=# ALTER USER postgres PASSWORD 'WqA1yT2z+!';
postgres=# \q
```

```
$ sudo -u postgres psql
postgres=# \password postgres
WqA1yT2z+!
postgres=# \q
```

### Create New Role

```
$ sudo -u postgres createuser --interactive
Enter name of role to add: windmill
Shall the new role be a superuser? (y/n) n
Shall the new role be allowed to create databases? (y/n) n
Shall the new role be allowed to create more new roles? (y/n) n

$ sudo -u postgres psql
postgres=# \password <user_role>
Enter new password for user "windmill": WqA1yT2z+
Enter it again: WqA1yT2z+
```

### Delete Role

```
$ sudo -u postgres psql
postgres=# drop role <username>;
postgres=# \l
postgres=# \q
```

### Create New Database

```
$ sudo -u postgres createdb <database> --owner <dbuser> --encoding=UTF8
$ sudo -u postgres createdb windmill --owner windmill --encoding=UTF8

$ sudo -u postgres psql
postgres=# grant all privileges on database <dbname> to <username>;
postgres=# grant all privileges on database windmill to windmill;
postgres=# \l
postgres=# \q 
```

### Delete Database

```
$ sudo -u postgres psql
postgres=# drop databse <dbname>;
postgres=# drop role <username>;
postgres=# \l
postgres=# \q
```

### Open PSQL with New Role

```
# if you create new role `windmill` with new database `windmill`
$ sudo adduser windmill
New password: WqA1yT2z+
Retype new password: WqA1yT2z+
passwd: password updated successfully

$ sudo -u windmill psql
windmill=> \conninfo

$ sudo -i -u windmill
$ psql
windmill=> \conninfo
```

### Create Role and Database via PSQL

#### Create User

```
$ sudo -u postgres psql
postgres=# create user <username> with encrypted password 'WqA1yT2z+';
postgres=# create database <dbname> with owner=<username> encoding='UTF8';
postgres=# grant all privileges on database <dbname> to <username>;
postgres=# drop databse <dbname>;
postgres=# drop role <username>;
postgres=# \l
postgres=# \q
```

#### Create Super User

```
$ sudo -u postgres psql
postgres=# CREATE ROLE <username> SUPERUSER LOGIN REPLICATION CREATEDB CREATEROLE;
postgres=# CREATE DATABASE <dbname> OWNER <USERNAME>;
postgres=# grant all privileges on database <dbname> to <username>;
postgres=# \password <username>
postgres=# DROP DATABASE <username>;
postgres=# DROP ROLE <username>;
postgres=# DROP USER <username>;
postgres=# \q
```

#### Create Backup User

```
$ sudo -u postgres psql
postgres=# create user <username> with password 'WqA1yT2z+' LOGIN REPLICATION;
postgres=# create database <dbname> with owner=<username> encoding='UTF8';
postgres=# grant all privileges on database <dbname> to <username>;
postgres=# drop databse <dbname>;
postgres=# drop role <username>;
postgres=# \l
postgres=# \q
```

### Check Authentication

```
$ psql --host=localhost --port=6336 --dbname=<dbname> --username=<username> --password
password: WqA1yT2z+
<dbname>=# \l
<dbname>=# \q
```

### Manage Role/User

#### List

```
postgres=# \du
postgres=# \du+
```

```
SELECT usename AS role_name,
  CASE 
     WHEN usesuper AND usecreatedb THEN 
	   CAST('superuser, create database' AS pg_catalog.text)
     WHEN usesuper THEN 
	    CAST('superuser' AS pg_catalog.text)
     WHEN usecreatedb THEN 
	    CAST('create database' AS pg_catalog.text)
     ELSE 
	    CAST('' AS pg_catalog.text)
  END role_attributes
FROM pg_catalog.pg_user
ORDER BY role_name desc;
```

### Manage Table

#### Create

```
postgres=# CREATE TABLE table_name (
postgres=#     column_name1 col_type (field_length) column_constraints,
postgres=#     column_name2 col_type (field_length),
postgres=#     column_name3 col_type (field_length)
postgres=# );

postgres=# CREATE TABLE playground (
postgres=#     equip_id serial PRIMARY KEY,
postgres=#     type varchar (50) NOT NULL,
postgres=#     color varchar (25) NOT NULL,
postgres=#     location varchar(25) check (location in ('north', 'south', 'west', 'east', 'northeast', 'southeast', 'southwest', 'northwest')),
postgres=#     install_date date
postgres=# );
```

#### List

```
postgres=# \d
postgres=# \dt
postgres=# \dt+
```

#### Update

```
postgres=# INSERT INTO playground (type, color, location, install_date) VALUES ('slide', 'blue', 'south', '2017-04-28');
postgres=# INSERT INTO playground (type, color, location, install_date) VALUES ('swing', 'yellow', 'northwest', '2018-08-16');

postgres=# SELECT * FROM playground;

# Add and delete column from table
postgres=# ALTER TABLE playground ADD last_maint date;
postgres=# ALTER TABLE playground DROP last_maint;

# Update data in table
postgres=# UPDATE playground SET color = 'red' WHERE type = 'swing';
```

#### Delete

```
postgres=# DELETE FROM playground WHERE type = 'slide';
```

#### Show tables using `pg_catalog` schema

```
postgres=# SELECT * FROM pg_catalog.pg_tables WHERE schemaname != 'pg_catalog' AND schemaname != 'information_schema';
```

### Manage Database

#### Switch Database

```
postgres=# \c <dbname>
```

### Manage Extension

#### List Installed Extensions

```
$ sudo apt search postgresql-contrib-16
$ ls /usr/share/postgresql/16/extension/
```

#### List Loaded Extensions

```
postgres=# SELECT * from pg_extension;
```

#### Load Installed Extension

```
postgres=# CREATE EXTENSION IF NOT EXISTS "citext";
postgres=# CREATE EXTENSION IF NOT EXISTS "pgcrypto";
postgres=# CREATE EXTENSION IF NOT EXISTS "uuid-ossp";
```

```
$ psql -U postgres -d <dbname> -f <module>.sql
```
