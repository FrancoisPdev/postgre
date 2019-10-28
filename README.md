# postgre
Hello everyone

[Tuto](#Tuto)(FR)

[Tips](#Tips)
![postegre](https://github.com/fanfanpsg/postgre/blob/master/postegre.png?raw=true)
how to start and use postgreSQL

1)
update/grade systeme
```
sudo apt update
sudo apt -y install vim bash-completion wget
sudo apt -y upgrade
```
2)
reboot
```
sudo reboot
```
3)
We need to import GPG key and add PostgreSQL 12 repository into our Ubuntu machine and After importing GPG key, add repository contents to your Ubuntu.
The repository added contains many different packages including third party addons. They include:

* postgresql-client
* postgresql
* libpq-dev
* postgresql-server-dev
* pgadmin packages

4)
*postgresql-12*(12 = version) postgresql-client-12
```
sudo apt update
sudo apt -y install postgresql-12 postgresql-client-12
```

5)
# The PostgreSQL service is started and set to come up after every system reboot.
(use 3 command)
```
systemctl status postgresql.service

systemctl status postgresql@12-main.service 

systemctl is-enabled postgresql
```
# Test PostgreSQL Connection
During installation, a postgres user is created automatically. This user has full superadmin access to your entire PostgreSQL instance. Before you switch to this account, your logged in system user should have sudo privileges.
```
sudo su - postgres
```
Let’s reset this user password to a strong Password we can remember.
```
psql -c "alter user postgres with password 'StrongAdminP@ssw0rd'"
```
start the repeal of postgre
```
psql
```
Get connection details like below.
```
psql (12.0 (Ubuntu 12.0-1.pgdg18.04+1))
Type "help" for help.
```
```
\conninfo
You are connected to database "postgres" as user "postgres" via socket in "/var/run/postgresql" at port "5432".
```
Let’s create a test database and user to see if it’s working.
```
postgres=# CREATE DATABASE mytestdb;
CREATE DATABASE
postgres=# CREATE USER mytestuser WITH ENCRYPTED PASSWORD 'MyStr0ngP@SS';
CREATE ROLE
postgres=# GRANT ALL PRIVILEGES ON DATABASE mytestdb to mytestuser;
GRANT
```

List created databases:
```
postgres=# \l
                               List of databases
   Name    |  Owner   | Encoding | Collate |  Ctype  |    Access privileges    
-----------+----------+----------+---------+---------+-------------------------
 mytestdb  | postgres | UTF8     | C.UTF-8 | C.UTF-8 | =Tc/postgres           +
           |          |          |         |         | postgres=CTc/postgres  +
           |          |          |         |         | mytestuser=CTc/postgres
 postgres  | postgres | UTF8     | C.UTF-8 | C.UTF-8 | 
 template0 | postgres | UTF8     | C.UTF-8 | C.UTF-8 | =c/postgres            +
           |          |          |         |         | postgres=CTc/postgres
 template1 | postgres | UTF8     | C.UTF-8 | C.UTF-8 | =c/postgres            +
           |          |          |         |         | postgres=CTc/postgres
(4 rows)
```
Connect to database:
```
\c mytestdb
You are now connected to database "mytestdb" as user "postgres".
```

Other PostgreSQL utilities installed such as createuser and createdb can be used to create database and users. (use "```quit```" if you are in ```postgres=#```.
```
postgres@ubuntu:~$ createuser myuser --password
Password:
postgres@ubuntu:~$ createdb mydb -O myuser
postgres@ubuntu:~$ psql -l 
```
### We can create and connect to a database on PostgreSQL server.
***
### Tuto

### Créer utilisateur PostgreSQL
Lorsqu’on vient d’un autre SGBD on ne sait pas forcément comment fonctionne PostgreSQL pour la création d’utilisateur.

En fait il possède des commandes et un utilisateur dédié. La première chose à faire va donc être de basculer sur cet utilisateur.

```
sudo -s -u postgres
```
Ensuite nous allons créer un nouvel utilisateur pour notre projet.

Il est toujours mieux d’isoler les rôles et de limiter la portée d’une base à un utilisateur.
```
createuser -d -P username
```
L’option -d va permettre à notre utilisateur de créer des bases. L’option -P va forcer l’affectation d’un mot de passe qui nous sera demandé de manière interactive.

### Créer la base de donnée

Nous allons maintenant créer la base de données en spécifiant que c’est mon utilisateur qui en est le possesseur (owner), d’où le -O. Cette opération se fait toujours depuis le shell postgres.
```
createdb -O bddname bdd
```
Connexion à la base de données PostgreSQL
Nous pouvons maintenant sortir du shell postgres et nous connecter avec notre utilisateur:
```
psql -U martin -h localhost bdd
```
À noter qu’il est possible de réaliser toutes ces opérations directement en SQL mais il est préférable d’utiliser les commandes dédiées.

En cas de doute n’hésitez pas utiliser ```--help``` sur les commandes précédentes.
***

### Tips

installing pgcli (for better view):
``` sudo apt install pgcli ```

If you are in the CLI, write : sudo -i -u postgre (= root )
you can see this :
```
postgres@yesweweb-X542UA:~$
```
Now you can surf in the folder and this is simply use : 
```
cd /etc/postgresql
```
after use ``` ls ``` and if you have 11 or 12 you have the version of postgre.

now you can find the folder who execute postegre :```usr/lib/postgresql/11/bin``` the files is ```pg_ctl```

### Start service of postgre

```
postgres@yesweweb-X542UA:/usr/lib/postgresql/11/bin$ service postgresql start
```
```
postgres@yesweweb-X542UA:/usr/lib/postgresql/11/bin$ service postgresql status
```
```
postgres@yesweweb-X542UA:/usr/lib/postgresql/11/bin$ ps auxf | grep postgres
```
### hba

Authorization of access: pg_hba

this one is the most important folder with other essential folder.
