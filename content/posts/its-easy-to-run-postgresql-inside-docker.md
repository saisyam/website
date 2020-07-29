+++
title = "It's easy to run PostgreSQL inside Docker"
description = "Our development involves databases and running PostgresQL inside Docker during the development phase saves lot of time for the developers."
author = "Saisyam"
publishdate = "2020-03-30"
lastmodified = "2020-06-09"
categories = ["databases"]
tags = [
    "postgres",
    "docker",
]
+++

[Docker](https://www.docker.com/) makes life easy to run most complex software without going through native installation process. I use docker to deliver development builds for the team to work in their local system. In this article we will see how to run [PostgreSQL](https://www.postgresql.org/) inside Docker container, connect to the database docker and run SQL scripts.    

## Introduction to Docker

> Docker is a set of platform as a service products that uses OS-level virtualization to deliver software in packages called containers. Containers are isolated from one another and bundle their own software, libraries and configuration files; they can communicate with each other through well-defined channels - Wikipedia

To learn more about Docker there is an excellent tutorial by [runnable](https://runnable.com/docker/). You can find any docker image (a software bundled inside a docker container is called image) from [docker hub](https://hub.docker.com/search/?type=image). The most popular ones are RabbitMQ, Nginx, Apache etc.

## Why PostgreSQL inside Docker

Our projects are based on client-server and microservices architecture. We extensively use databases like, MySQL, PostgreSQL and MongoDB. We use messaging queues like RabbitMQ and Redis. So we encourage our developers to use dockers instead of installing all the software on their laptops.

In this article we will discuss on how to use PostgreSQL docker. We cover the following points:

1. Running the PostgreSQL docker
2. Logging into the docker container
3. Logging into PostgreSQL with `psql` shell
4. Copying SQL files to docker and running them

**Note:** You need to have docker installed on your computer or laptop. For installing Docker community edition refer [here](https://docs.docker.com/install/).

{{< adsense type="article" >}}

## Running the PostgreSQL docker

Use the following command to run PostgreSQL docker:

`$ docker run --name postgres-docker -e POSTGRES_PASSWORD=postgres -p 5432:5432 -d postgres`

1. Pulls the latest Postgres Docker image from Docker Hub.
2. Sets the POSTGRES_PASSWORD environment variable value to `postgres`.
3. Names (--name) the Docker container to be `postgres-docker`.
4. Maps container's internal 5432 port to external 5432 port, so we will be able to enter it from outside.
5. Run the Docker container in the background (-d).

**Note:** In case you stopped your docker or restarted your computer or laptop you can restart your docker by its name. To get the list of containers available (both running and stopped) use the following command:

`$ docker ps -a`

To start your docker again use the following command:

`$ docker start <name of the docker>`

This is especially helpful when starting/stopping database dockers, as we store data (tables and records) in our docker containers.

{{< adsense type="article" >}}

## Logging into the docker container

You can login into the docker container with `docker exec` command. Use below command to login as `root` into the running docker container:


`$ docker exec -it <name of the docker> bash`

`$ root@377ef2b9b13e:/#`

The name of the docker in our case is, `postgres-docker`. You will get to the prompt logged in as `root`.

## Log into PostgreSQL inside Docker

Once you are into the bash shell of the docker you can use `psql` command to manage your PostgreSQL database. You the following command to manage your database:

{{< gist saisyam 6e9e54aed3380dc6c6e6fd54bb14b792 >}}

We are using the default `postgres` user. You will get to the prompt to run commands like:

1. `\l` to list the available databases
2. `\c <database name>` to select the database

Run SQL commands like, `SELECT`, `INSERT`, `DELETE`, `UPDATE` etc. to manipulate your tables and records. Refer to `psql` documentation [here](https://www.postgresql.org/docs/10/app-psql.html).


## Copying SQL files to PostgreSQL inside Docker

Sometime we will have an existing schema (SQL file) which we need to execute once we have the docker up and running. To do that we need to copy our SQL file into the docker and then run the SQL file.

Copy the SQL file using `docker cp` command:

`$ docker cp ./world.sql projectm-postgres:/docker-entrypoint-initdb.d/world.sql`

Interpret the above command:

`$ docker cp <sql file> <docker name>:/<path in docker container>/<sql file>`

We will execute the SQL file using `docker exec` command:

`$ docker exec -u postgres projectm-postgres psql world postgres -f docker-entrypoint-initdb.d/world.sql`

Interpret the above command:

`$ docker exec -u <postgres user> <docker name> psql <dbname> <postgres user> -f <path to SQL file>`

The file I uses contains a `CREATE TABLE` and `INSERT TABLE` command. So the result says, `CREATE TABLE` successful and inserted 239 records. The SQL file I used can be downloaded from [here](https://gist.github.com/saisyam/2859fcc386e7405d1416a67c6c8e859a)

{{< adsense type="article" >}}

## Conclusion
Running databases within Docker provides a lot of flexibility by avoiding the complex installation procedure. Thanks for reading. I will come up with an another article using docker-compose.
