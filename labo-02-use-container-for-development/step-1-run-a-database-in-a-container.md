# Step 1 - Run a database in a container

* [Official Source](https://docs.docker.com/language/java/develop/#run-a-database-in-a-container)

## Prerequisites

Take the time to become familiar with the concept of volumes before carrying out this lab : [Docker and Volumes](https://docs.docker.com/storage/volumes/).

## Setup Database Docker dependencies

### Add volumes

Let's create both volumes, one for the data, and another one for the db config.

* [x] Create one volume for the MySQL data (tag name : mysql_data)

```
[INPUT]
docker volume create mysql_data

[OUTPUT]
mysql_data
```

* [x] Create a second volume for the MySQL configuration (tag name : mysql_config)

```
[INPUT]
docker volume create mysql_config

[OUTPUT]
mysql_config
```

* [x] List the volumes

```
[INPUT]
docker volume ls

[OUTPUT]
DRIVER    VOLUME NAME
local     mysql_config
local     mysql_data
local     volume-ria1-shopping
local     vsCodeServerVolume-ria1-shopping-app
```

### Network Creation

Let's create a user-defined bridge network enabling our application and our database to talk to each other.

* [ ] Create the network

```
[INPUT]
docker network create mysqlnet

[OUTPUT]
17332d080d53245289f6ebbe243aff8ec071dba7dece58338575b3d340b944bb
```

* [x] List the networks

```
[INPUT]
docker network ls

[OUTPUT]
NETWORK ID     NAME       DRIVER    SCOPE
93d366eebb0d   bridge     bridge    local
68b18fa5cf2f   host       host      local
17332d080d53   mysqlnet   bridge    local
343084e056ed   none       null      local
```

### Run MySQL

Let's run MySQL in a container and attach to the volumes and network we created above.

{% hint style="info" %}
To avoid this message:
```
docker: Error response from daemon: Ports are not available: exposing port TCP 0.0.0.0:3306 -> 0.0.0.0:0: listen tcp 0.0.0.0:3306: bind: Only one usage of each socket address (protocol/network address/port) is normally permitted.
```
Check your host ports and do no try to forward one of them that is already is use.
{% endhint %}

```
[INPUT]
docker run -it --rm -d -v mysql_data:/var/lib/mysql -v mysql_config:/etc/mysql/conf.d --network mysqlnet --name mysqlserver -e MYSQL_USER=petclinic -e MYSQL_PASSWORD=petclinic -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=petclinic -p 3306:3306 mysql:8.0

docker build --tag java-spring:dev .\

docker run --rm -d --name springboot-server --network mysqlnet -e MYSQL_URL=jdbc:mysql://mysqlserver/petclinic -p 8080:8080 java-spring:dev
//TODO Run docker

[OUTPUT]
Unable to find image 'mysql:8.0' locally
8.0: Pulling from library/mysql
328ba678bf27: Downloading [====>                                              ]  4.107MB/44.56MB
f3f5ff008d73: Download complete
dd7054d6d0c7: Download complete
70b5d4e8750e: Downloading [======================================>            ]   3.55MB/4.608MB
cdc4a7b43bdd: Download complete
a0608f8959e0: Download complete
5823e721608f: Downloading [>                                                  ]  1.081MB/58.59MB
a564ada930a9: Waiting
539565d00e89: Waiting
a11a06843fd5: Waiting
92f6d4aa041d: Waiting
[...]
2b7afc93c37d715c6d592de78173386903e123d0c7065ac34329ab81e9fcefd8
```

* [ ] List all containers (all states)

```
[INPUT]
//TODO

[OUTPUT]
//TODO Result expected
IMAGE                              PORTS.                               NAMES
mysql:8.0                          33060/tcp, 0.0.0.0:3316->3306/tcp.   mysqlserver
eclipse-petclinic:version1.0.dev   0.0.0.0:80->8080/tcp.                petclinic-server
```

### Update our Dockerfile to activate MySQL

Currently H2 is used on our Petclinic Container. We need to switch on MySQL.

* [ ] Add this command to your Dockerfile, in the right place.

```
CMD ["./mvnw", "spring-boot:run", "-Dspring-boot.run.profiles=mysql"]
```

* [ ] If you run both docker, the application server will not able to talk with the dbserver... any idea why ?

```
//TODO
```

* [ ] Let's build our image

```
[INPUT]
docker build --tag eclipse-petclinic:version1.1.dev .

[OUTPUT]
//Result Expected (version1.1.dev)
docker images
REPOSITORY          TAG              IMAGE ID       CREATED         SIZE
eclipse-petclinic   version1.1.dev   7dcc2df873a9   6 seconds ago   607MB
eclipse-petclinic   version1.0.dev   323bdb488603   4 days ago      606MB
eclipse-temurin     17-jdk-jammy     56c7bc12ee6d   13 days ago     456MB
mysql               8.0              8189e588b0e8   4 weeks ago     564MB
```

* [ ] Test your application

```
[INPUT]
//TODO Start your docker


[INPUT]
//Call the vets route
curl --request GET ^
    --url http://localhost/vets ^
    --header 'content-type: application/json'

[OUTPUT]
//Result Expected
```
