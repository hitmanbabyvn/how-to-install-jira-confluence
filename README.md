### Install PostgreSQL  

```bash
sudo yum install postgresql-server postgresql-contrib
```
Initialize your PostgreSQL database and start postgresql

```bash
sudo postgresql-setup initdb
```

By default, PostgreSQL does not allow password authentication.
Open the HBA configuration


```bash
sudo nano /var/lib/pgsql/data/pg_hba.conf
```

Find

```
host    all             all             127.0.0.1/32            ident
host    all             all             ::1/128                 ident
```

Then replace "ident" with "md5"

```
host    all             all             127.0.0.1/32            md5
host    all             all             ::1/128                 md5
```

Start and enable PostgreSQL

```bash
systemctl start postgresql
systemctl enable postgresql
```

### Create PostgreSQL user and database

By default, Postgres uses a concept called "roles" to aid in authentication and authorization. These are, in some ways, similar to regular Unix-style accounts, but Postgres does not distinguish between users and groups and instead prefers the more flexible term "role".

```
sudo -i -u postgres
createuser jira
createuser confluence
createdb jira
createdb confluence
```

Grant access to users

```bash
psql
alter user <username> with encrypted password 'password';
grant all privileges on database <database> to <username>;
```

### Install JIRA & CONFLUENCE

Download JIRA https://www.atlassian.com/software/jira/download
Download CONFLUENCE https://www.atlassian.com/software/confluence/download

Make the installer executable and run

```bash
chmod a+x atlassian-jira-software-X.X.X-x64.bin
sudo ./atlassian-jira-software-X.X.X-x64.bin
```

Follow the promt to install JIRA/ CONFLUENCE

- Install type – choose option 2 (custom) for the most control.
- Destination directory – this is where JIRA will be installed.
- Home directory – this is where JIRA data like logs, search indexes and files will be stored.
- TCP ports – these are the HTTP connector port and control port JIRA will run on. Stick with the default unless you're running another application on the same port.
- Install as service – this option is only available if you ran the installer as sudo.

Once installation is complete head to

- JIRA http://localhost:8080
- CONFLUENCE http://localhost:8090

in your browser to begin the setup process.

### CREDIT
- https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-centos-7
- https://medium.com/coding-blocks/creating-user-database-and-adding-access-on-postgresql-8bfcd2f4a91e
