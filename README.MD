# Bootstrap a new project using docker
More on this [HERE](https://www.digitalocean.com/community/tutorials/how-to-set-up-laravel-nginx-and-mysql-with-docker-compose).

## Create a new laravel application
Use an ephemeral docker container to run composer without having to install it globally on your machine.

```bash
docker run --rm -v $(pwd):/app composer create-project --prefer-dist laravel/laravel my-project

sudo chown -R $USER:$USER my-project
``` 

## Use docker to setup the dev environment
The needed services are described in the docker-compose.yml file.

Notice that there are some files/directories that are binded to containers in the different services for persistence, using volumes. They are basically config files and storage for the database.

Copy the different config files to the newly created laravel project including :
```
/
| -- mysql/
| -- nginx/
| -- php/
| -- docker-compose.yml
| -- Dockerfile
```
Go through the docker-compose file to rename the docker image that will be generated for the app and the containers, but most importantly, update the db service environment variables to configure your mysql database.

```
environment:
      MYSQL_DATABASE: mydb
      MYSQL_ROOT_PASSWORD: rootpasswd
      MYSQL_USER: myuser
      MYSQL_PASSWORD: myuserpasswd
      SERVICE_TAGS: dev
      SERVICE_NAME: mysql
```  

Update the .env file to configure Laravel connection to the database.

```
DB_CONNECTION=mysql
DB_HOST=db
DB_PORT=3306
DB_DATABASE=mydb
DB_USERNAME=myuser
DB_PASSWORD=myuserpasswd
``` 
`DB_HOST` is your db container name.

## Connect to different containers 
To use `php artisan` you may want to connect to your app container. You can execute an interactive shell on the container.

```bash
docker-compose exec app bash
``` 

The same works for your database container, just replace `app` by `db`.