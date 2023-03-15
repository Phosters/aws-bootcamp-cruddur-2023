# Week 4 — Postgres and RDS

### Create postgresql in AWS
This week focuses on RDS with focus on Postgress
We will be using RDS from AWS, so lets create a ```postgresql ``` in AWS with name ``` cruddur-db-instance ```

### Now we will create in our development envt a postgress db
First connect to postgresql client to work within postgress

```psql -U postgres --host localhost
```

Check whether db to be created name exist already with this

```
\l   
```

then use this to quit

```
\q
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

Now we need to have Postgres generate out UUIDs. We'll need to use an extension called:

```
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";

```

Now lets export the connection url to avoid inserting password everytime we login

```
export CONNECTION_URL="postgresql://postgres:password@localhost:5432/cruddur"
```

and set it for our env too 

```
gp env CONNECTION_URL="postgresql://postgres:password@localhost:5432/cruddur"

```

Once this is done, we will want to control our db with scripting
lets locate our backend and create a ``` bin ``` folder and add three files with name ``` db-drop ``` , ``` db-create ``` , and ```db-schema load ```
to access them be in the backedend and  type ``` ls -al ./bin ``` to show the three files which wont give access to them, so letsgive them the preferred permission.

```
chmod 644 ./bin/db-drop

```

```
chmod 644 ./bin/db-schema-load

```

```
chmod 644 ./bin/db-create

```

Now to manipulate strings, we will be using SED which is a text stream editor used on Unix systems to edit files quickly and efficiently. The tool searches through, replaces, adds, and deletes lines in a text file without opening the file in a text editor

```
echo "db-drop"

NO_DB_CONNECTION_URL=$(sed 's/\/cruddur//g' <<<"$CONNECTION_URL")

psql $NO_DB_CONNECTION_URL -c "drop database cruddur;"

```
to create the deleted db again 

```
echo "db-create"

NO_DB_CONNECTION_URL=$(sed 's/\/cruddur//g' <<<"$CONNECTION_URL")

psql $NO_DB_CONNECTION_URL -c "create database cruddur;"

```



