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
chmod u + x ./bin/db-drop

```

```
chmod u + x ./bin/db-schema-load

```

```
chmod u + x ./bin/db-create

```

Now to manipulate strings, we will be using SED which is a text stream editor used on Unix systems to edit files quickly and efficiently. The tool searches through, replaces, adds, and deletes lines in a text file without opening the file in a text editor

```
echo "db-drop"

NO_DB_CONNECTION_URL=$(sed 's/\/cruddur//g' <<<"$CONNECTION_URL")

psql $NO_DB_CONNECTION_URL -c "drop database cruddur;"

```
to create the deleted db again insert this command to create a new db cruddur within the db/db-create file

```
echo "db-create"

NO_DB_CONNECTION_URL=$(sed 's/\/cruddur//g' <<<"$CONNECTION_URL")

psql $NO_DB_CONNECTION_URL -c "create database cruddur;"

```

Now we will need a centralised file to manupulate the entire bin files from, from create to drop to connect to schema load, we create a file in bin called db-setup and insert this commands

```
#! /usr/bin/bash
-e # stop if it fails at any point

#echo "==== db-setup"
CYAN='\033[1;36m'
NO_COLOR='\033[0m'
LABEL="db-setup"
printf "${CYAN}==== ${LABEL}${NO_COLOR}\n"

bin_path="$(realpath .)/bin"

source "$bin_path/db-drop"
source "$bin_path/db-create"
source "$bin_path/db-schema-load"
source "$bin_path/db-seed"

```

make sure the set-up file is executeable with this

```
chmod u+x ./bin/db-setup

```

lets install python packages for postgres in the bcackend within the requirements file with pscopg because Psycopg is the most popular PostgreSQL adapter for the Python programming language. Its core is a complete implementation of the Python DB API 2.0 specifications. Several extensions allow access to many of the features offered by PostgreSQL. this will facilitate 

```
psycopg[binary]
psycopg[pool]

``

to implement this we need to install the requirement file to take effect

```
pip install -r requirements.txt

```

now we do a Database connection pooling which is a way to reduce the cost of opening and closing connections by maintaining a “pool” of open connections that can be passed from database operation to database operation as needed
we start with creating a a file in lib at the backend with this

```
lib/db.py

```

after that, we will then create our connection pool with this within the db.py file

```
from psycopg_pool import ConnectionPool
import os

def query_wrap_object(template):
  sql = '''
  (SELECT COALESCE(row_to_json(object_row),'{}'::json) FROM (
  {template}
  ) object_row);
  '''

def query_wrap_array(template):
  sql = '''
  (SELECT COALESCE(array_to_json(array_agg(row_to_json(array_row))),'[]'::json) FROM (
  {template}
  ) array_row);
  '''

connection_url = os.getenv("CONNECTION_URL")
pool = ConnectionPool(connection_url)

```

now after setting our connection url, we will add it to our docker compose file backend environment

``
CONNECTION_URL: "postgresql://postgres:password@db:5432/cruddur"

```











