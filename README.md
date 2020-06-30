# How to set up n8n on Clever Cloud

1. create a new NodeJs application linked to a fork of this repo
2. in your application environment variables add ```N8N_HOST=n8n.example.com``` (don't use the cleverapps domain in production)
3. you can now build and start the application, be aware however that if you restart it your data will not be persisted (see next section to learn how to persist data)

# Persisting Data on a database

For this example we will use postgresql however the procedure shouldn't be too different with any other type of database supported by n8n.

1. create your database add-on on clever cloud and link it to the application
2. in your application environment variables you should now see the info you need to connect to your db

# How it works

1. create a new repo with a package.json with at least:
    ```
{
    "name": "n8n-clever-cloud",
    "version": "1.0.0",
    "description": "n8n on clevercloud",
    "dependencies": {
        "n8n": "0.71.0"
    },
    "scripts" : {
        "install" : "./build.sh",
        "start" : "./run.sh"
    },
    "author": "Loic Tosser <wowi42>",
    "license": "ISC",
    "bugs": {
        "url": "https://github.com/wowi42/n8n-clever-cloud/issues"
    },
    "homepage": "https://github.com/wowi42/n8n-clever-cloud#readme",
    "engines": {
        "node": "^14"
    }
}
    ```
2. then create both scripts, the install one will build n8n and the start one will start the n8n-server binary
3. in the install script put:
    ```
    #!/bin/bash
    set -e
    set -x
	
    npm install
    ```
4. in the start script put:
    ```
    #!/bin/bash
    set -e
    set -x
	
    export N8N_PORT=$PORT
    export N8N_PROTOCOL=https
    export DB_TYPE=postgresdb
    export DB_POSTGRESDB_DATABASE=$POSTGRESQL_ADDON_DB
    export DB_POSTGRESDB_HOST=$POSTGRESQL_ADDON_HOST
    export DB_POSTGRESDB_PORT=$POSTGRESQL_ADDON_PORT
    export DB_POSTGRESDB_USER=$POSTGRESQL_ADDON_USER
    export DB_POSTGRESDB_PASSWORD=POSTGRESQL_ADDON_PASSWORD
    export DB_POSTGRESDB_SCHEMA=n8n
    ./node_modules/.bin/n8n start
    ```
5. n8n is now ready to start
