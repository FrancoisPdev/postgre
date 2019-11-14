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

***
Access the PostgreSQL server from psql with a specific user:

`psql -U [username];`

* For example, the following command uses the postgres user to access the PostgreSQL database server:

  * `psql -U postgres`

Connect to a specific database:

`\c database_name;`

For example, the following command connects to the dvdrental database:

`\c dvdrental;
You are now connected to database "dvdrental" as user "postgres".`

To quit the psql:

`\q`

List all databases in the PostgreSQL database server:

`\l`

List all schemas:

`\dn`

List all stored procedures and functions:

`\df`

Lis all views:

`\dv`

Lists all tables in a current database:

`\dt`

Or to get more information on tables in the current database:

`\dt+`

Get detailed information on a table:

`\d+ table_name`

Show a stored procedure or function code:

`\df+ function_name`

Show query output in the pretty-format:

`\x`

List all users:

`\du`

Create a new role:

`CREATE ROLE role_name;`

Create a new role with a username and password:

`CREATE ROLE username NOINHERIT LOGIN PASSWORD password;
`
Change role for the current session to the new_role:

`SET ROLE new_role;`

Allow role_1 to set its role as role_2:

`GRANT role_2 TO role_1;`

## Managing databases

Create a new database:

`CREATE DATABASE [IF NOT EXISTS] db_name;`

Delete a database permanently:

`DROP DATABASE [IF EXISTS] db_name;`

## Managing tables
Create a new table or a temporary table

```CREATE [TEMP] TABLE [IF NOT EXISTS] table_name(
   pk SERIAL PRIMARY KEY,
   c1 type(size) NOT NULL,
   c2 type(size) NULL,
   ...
);
```
Add a new column to a table:

`
ALTER TABLE table_name ADD COLUMN new_column_name TYPE;
`

Drop a column in a table:

`
ALTER TABLE table_name DROP COLUMN column_name;
`

Rename a column:

`
ALTER TABLE table_name RENAME column_name TO new_column_name;
`

Set or remove a default value for a column:

`
ALTER TABLE table_name ALTER COLUMN [SET DEFAULT value | DROP DEFAULT]
`

Add a primary key to a table:

`
ALTER TABLE table_name ADD PRIMARY KEY (column,...);
`

Remove the primary key from a table:

`
ALTER TABLE table_name 
DROP CONSTRAINT primary_key_constraint_name;
`

Rename a table:

`
ALTER TABLE table_name RENAME TO new_table_name;
`

Drop a table and its dependent objects:

`
 DROP TABLE [IF EXISTS] table_name CASCADE;
`

## Managing views
Create a view:

`
CREATE OR REPLACE view_name AS
query;
`

Create a recursive view:

`
CREATE RECURSIVE VIEW view_name(columns) AS
SELECT columns;
`

Create a materialized view:

`
CREATE MATERIALIZED VIEW view_name
AS
query
WITH [NO] DATA;
`

Refresh a materialized view:

`
REFRESH MATERIALIZED VIEW CONCURRENTLY view_name;
`

Drop a view:

`
DROP VIEW [ IF EXISTS ] view_name;
`

Drop a materialized view:

`
DROP MATERIALIZED VIEW view_name;
`

Rename a view:

`
ALTER VIEW view_name RENAME TO new_name;
`

## Managing indexes

Creating an index with the specified name on a table:

`
CREATE [UNIQUE] INDEX index_name
ON table (column,...)
`

Removing a specified index from a table:

`
DROP INDEX index_name;
`

## Querying data from tables

Query all data from a table:

`
SELECT * FROM table_name;
`

Query data from specified columns of all rows in a table:

`
SELECT column, column2….
FROM table;
`

Query data and select only unique rows:

`
SELECT DISTINCT (column)
FROM table;
`

Query data from a table with a filter:

`
SELECT *
FROM table
WHERE condition;
`

Assign an alias to a column in the result set:

`
SELECT column_1 AS new_column_1, ...
FROM table;
`

Query data using the `LIKE` operator:

`
SELECT * FROM table_name
WHERE column LIKE '%value%'
`

Query data using the BETWEEN operator:

`
SELECT * FROM table_name
WHERE column BETWEEN low AND high;
`

Query data using the `IN` operator:

`
SELECT * FROM table_name
WHERE column IN (value1, value2,...);
`

Constrain the returned rows with the `LIMIT` clause:

`
SELECT * FROM table_name
LIMIT limit OFFSET offset
ORDER BY column_name;
`

Query data from multiple using the inner join, left join, full outer join, cross join and natural join:

`
SELECT * 
FROM table1
INNER JOIN table2 ON conditions
`

`
SELECT * 
FROM table1
LEFT JOIN table2 ON conditions
`

`
SELECT * 
FROM table1
FULL OUTER JOIN table2 ON conditions
`

`
SELECT * 
FROM table1
CROSS JOIN table2;
`

`
SELECT * 
FROM table1
NATURAL JOIN table2;
`

Return the number of rows of a table:

`
SELECT COUNT (*)
FROM table_name;
`

Sort rows in ascending or descending order:

`
SELECT column, column2, ...
FROM table
ORDER BY column ASC [DESC], column2 ASC [DESC],...;
`

Group rows using GROUP BY clause:

`
SELECT *
FROM table
GROUP BY column_1, column_2, ...;
`

Filter groups using the HAVING clause:

`
SELECT *
FROM table
GROUP BY column_1
HAVING condition;
`

## Set operations

Combine the result set of two or more queries with UNION operator:

`
SELECT * FROM table1
UNION
SELECT * FROM table2;
`

Minus a result set using `EXCEPT` operator:

`
SELECT * FROM table1
EXCEPT
SELECT * FROM table2;
`

Get intersection of the result sets of two queries:

`
SELECT * FROM table1
INTERSECT
SELECT * FROM table2;
`

## Modifying data

Insert a new row into a table:

`
INSERT INTO table(column1,column2,...)
VALUES(value_1,value_2,...);
`

Insert multiple rows into a table:

`
INSERT INTO table_name(column1,column2,...)
VALUES(value_1,value_2,...),
      (value_1,value_2,...),
      (value_1,value_2,...)...
`

Update data for all rows:

`
UPDATE table_name
SET column_1 = value_1,
    ...;
`

Update data for a set of rows specified by a condition in the `WHERE` clause:

`
UPDATE table
SET column_1 = value_1,
    ...
WHERE condition;
`

Delete all rows of a table:

`
DELETE FROM table_name;
`

Delete specific rows based on a condition:

`
DELETE FROM table_name
WHERE condition;
`

## Performance

Show the query plan for a query:

`
EXPLAIN query;
`

Show and execute the query plan for a query:

`
EXPLAIN ANALYZE query;
`

Collect statistics:

`
ANALYZE table_name;
`







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
