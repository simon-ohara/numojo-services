Database users

* numojo-root   Root
* numojo-admin  Admin (Mongo Express user)
* numojo-api    API (api service user)

Database Secrets

* DB_HOST=mongodb://mongo
* DB_PORT=27017
* DB_NAME=numojo
* DB_AUTH=numojo-auth # authentication db

* DB_ROOT_USER=numojo-root
* DB_ROOT_PASS=secret 

* DB_ADMIN_USER=numojo-admin
* DB_ADMIN_PASS=secret

* DB_API_USER=numojo-api
* DB_API_PASS=secret

How to add users to mongodb service

- https://stackoverflow.com/a/62751882/2683193
- https://hub.docker.com/_/mongo - /Environment variables
