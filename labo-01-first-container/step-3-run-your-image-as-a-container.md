# Step 3 - Run your image as a container

* [Official Source](https://docs.docker.com/language/java/run-containers/)

<!---->

* [ ] Run your "petclinic" docker based on the image created in the previous step.
  * [ ] We should access to your application using the http standard port.
```bash
docker run -d -p 80:8080 --name petclinic java-spring:dev
```

Result expected:

{% hint style="info" %}
Linux user, change ^ by ' to execute multi lines commands.
{% endhint %}

```
[INPUT]
curl --request GET ^
--url http://localhost/actuator/health ^
--header 'content-type: application/json'

[OUTPUT]
{"status":"UP"}

//disregard the message curl: (6) Could not resolve host: application
```

* [x] List all Dockers currently running on your local environment. Observe the port forwarding for your "petclinic" docker.

```
[INPUT]
docker ps

[OUTPUT]
CONTAINER ID   IMAGE             COMMAND                  CREATED         STATUS         PORTS                  NAMES
35bfdf5f034fe   java-spring:dev   "./mvnw spring-boot:…"   12 minutes ago   Up 2 minutes   0.0.0.0:80->8080/tcp   petclinic
```

* [ ] Stop your "petclinic" docker

```
[INPUT]
docker stop petclinic

[OUTPUT]
petclinic
```

* [x] Rename your docker as "petclinic-app"

<!---->

* [Official doc](https://docs.docker.com/engine/reference/commandline/rename/)

```
[INPUT]
docker rename petclinic petclinic-app

[OUTPUT]
//TODO
```

* [ ] Restart your docker using the new name

```
[INPUT]
docker restart petclinic-app

[OUTPUT]
petclinic-app
```

* [x] Display all running dockers with this output format

<!---->

* [Official doc](https://docs.docker.com/config/formatting/)

Result expected:

```
IMAGE                              PORTS.                  NAMES
eclipse-petclinic:version1.0.dev   0.0.0.0:80->8080/tcp.   petclinic-server
```

```
[INPUT]
//TODO

[OUTPUT]
java-spring:dev   0.0.0.0:80->8080/tcp   petclinic-app
```

