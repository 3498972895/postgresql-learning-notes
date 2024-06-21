# Setup Postgresql Ubuntu

## installation

```bash
sudo apt install postgresql
```

## login postgresql

When we have been installed postgresql, So We can login in postgre with terminal way. 

The default account of the postgresql server provided is postgres, the database already existing is  postgres. The psql stands for the interactive terminal client.

```bash
sudo -u postgres psql postgres
```

Then We should create a user 'fukuda'(what ever you like) with superuser previlige by using:

```sql
CREATE USER fukuda SUPERUSER;
```

The postgresql requires us specific the name of database when we login, So We better do create A New database  named first_database: 

```sql
create database first_database;
```

And typing '\\q' for quit, and Login Again with blew commands for making sure everything goes well.

```bash
psql -U fukuda -d first_database
```


