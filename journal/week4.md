# Week 4 — Postgres and RDS

### Create postgresql in AWS
This week focuses on RDS with focus on Postgress
We will be using RDS from AWS, so lets create a ```postgresql ``` in AWS with name ``` cruddur-db-instance ```

### Now we will create in our development envt a postgress db
First connect to postgresql client to work within postgress

```
psql -U postgres --host localhost

```
Check whether db to be created name exist already with this

```
\l   

```

then use this to quit

```
q

```

We create postgresql with this command 

```
CREATE database cruddur;

```

Now lets import the script of the db
We'll create a new SQL file called schema.sql and we'll place it in backend-flask/db
The command to import:

```
psql cruddur < db/schema.sql -h localhost -U postgres

```
