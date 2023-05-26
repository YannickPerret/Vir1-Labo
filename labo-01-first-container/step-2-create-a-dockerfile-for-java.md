# Step 2 - Create a Dockerfile for Java

* [Official Source](https://docs.docker.com/language/java/build-images/#create-a-dockerfile-for-java)

## Create your docker file

* [x] Change the directory to the app directory (we will use the same project [as for the previous step](step-1-run-the-project-outside-docker.md)).
* [x] Create an empty file named "Dockerfile"

```
[INPUT]
touch Dockerfile

[OUTPUT]

```

* Using your IDE, add the following contents to the Dockerfile:

```
# syntax=docker/dockerfile:1

FROM eclipse-temurin:17-jdk-jammy
```

* [x] What is the purpose of the directive starting with "# synthax=docker..."

<!---->

* [Official documentation - Syntax directive](https://docs.docker.com/build/dockerfile/frontend/)

```
Elle indique à Docker l'image de base qu'on souhaite utiliser pour notre application
```

* [x] Is the docker image suitable for a production environment?

```
Non, elle ne lance pas encore notre application
```

### Dependencies resolution and first app build

* [x] Set the image's working directory by adding this command line:

```
WORKDIR /app
```

* [x] Before we can resolve dependencies with _MAVEN,_ we need to get the _MAVEN_ wrapper and your pom.xml file into your image.

```
COPY .mvn/ .mvn
COPY mvnw pom.xml ./
```

* [x] Let's use _MAVEN_ to resolve the dependencies.

```
RUN ./mvnw dependency:resolve
```

* [x] How is it possible to resolve the dependencies when the source code is not available at this point in the process?

```
Les dépendances sont lister dans le fichier pom.xml
```

* Add the command able to provide (copy) your source code to the image.

```
COPY src ./src
```

* Add the command responsible to run your application inside the Docker.

```
CMD ["./mvnw", "spring-boot:run"]
```

## Create a .dockerignore file

* [Official documentation - dockerignore](https://docs.docker.com/language/java/build-images/#create-a-dockerignore-file)

{% hint style="info" %}
To meet the good practice provided by Docker, we have to reduce the amount of data in the Docker build context. For this first lab, we will simply remove the directory containing the eventual outputs of MAVEN.
{% endhint %}

* [x] Create a .dockerignore file in the same directory as the Dockerfile.
* [x] Add the folder containing the MAVEN output.

## Build an image

{% hint style="info" %}
Best practices - docker tag naming convention:\
Read carefully [this doc](https://docs.docker.com/develop/dev-best-practices/) and deduce the right tag for your image.\
This [doc may also help you with tag samples](https://docs.docker.com/engine/reference/commandline/tag/).
{% endhint %}

* [x] Find the best tag for your image (this image is not intended for publication.)

```
java-spring:dev
```

* [x] Build your first Docker image

Result Expected:

```docker
[INPUT]
docker build --tag <repository:tag>

[OUTPUT]
Sending build context to Docker daemon   9.92MB
Step 1/7 : FROM eclipse-temurin:17-jdk-jammy
 ---> 56c7bc12ee6d
 ---> Using cache
 ---> b4854585fa31
Step 3/7 : COPY .mvn/ .mvn
 ---> Using cache
 ---> b72a055eb5f8
Step 4/7 : COPY mvnw pom.xml ./
 ---> Using cache
 ---> ae3709e4bfb7
Step 5/7 : RUN ./mvnw dependency:resolve
 ---> Using cache
 ---> e62d33f55785
Step 6/7 : COPY src ./src
 ---> Using cache
 ---> b926a101aec1
Step 7/7 : CMD ["./mvnw", "spring-boot:run"]
 ---> Using cache
 ---> 910e5f0c8b1f
Successfully built <yourImageId>
Successfully tagged <yourRepository:yourTag>
SECURITY WARNING: You are building a Docker image from Windows against a non-Windows Docker host. All files and directories added to build context will have '-rwxr-xr-x' permissions. It is recommended to dou
ble check and reset permissions for sensitive files and directories.

```

```
[INPUT]
docker build --tag java-spring:dev .\

[OUTPUT]
[+] Building 331.6s (11/11) FINISHED                                                                                                                                                                                                          
 => [internal] load build definition from Dockerfile                                                                                                                                                                                     0.0s 
 => => transferring dockerfile: 238B                                                                                                                                                                                                     0.0s 
 => [internal] load .dockerignore                                                                                                                                                                                                        0.0s 
 => => transferring context: 47B                                                                                                                                                                                                         0.0s 
 => [internal] load metadata for docker.io/library/eclipse-temurin:17-jdk-jammy                                                                                                                                                          1.9s 
 => [1/6] FROM docker.io/library/eclipse-temurin:17-jdk-jammy@sha256:13ff0fa4f9232f69b9f6e8a4cf86b7cb3a3783dbee06818aa5aee447159c1bc3                                                                                                   65.6s 
 => => resolve docker.io/library/eclipse-temurin:17-jdk-jammy@sha256:13ff0fa4f9232f69b9f6e8a4cf86b7cb3a3783dbee06818aa5aee447159c1bc3                                                                                                    0.0s 
 => => sha256:c24bf4c725c221ddbb66e056716cf961f79001fedf2e8a0cddab347244398bac 192.59MB / 192.59MB                                                                                                                                      64.0s 
 => => sha256:13ff0fa4f9232f69b9f6e8a4cf86b7cb3a3783dbee06818aa5aee447159c1bc3 1.21kB / 1.21kB                                                                                                                                           0.0s 
 => => sha256:9dd6a19e4819b066aa2bd8e54d5988a49cca29736fe5447cb0a57daa975f8935 1.16kB / 1.16kB                                                                                                                                           0.0s 
 => => sha256:56c7bc12ee6d61d5e12e1fe0bd6ae1ebc31acd94b3c4fbbf65efe17fff910a29 6.31kB / 6.31kB                                                                                                                                           0.0s 
 => => sha256:1bc677758ad7fa4503417ae5be18809c5a8679b5b36fcd1464d5a8e41cb13305 30.43MB / 30.43MB                                                                                                                                        19.9s 
 => => sha256:0d0e0ecb256ae3e8b7625494ed35189f845766552b7159a17f634706e28a9687 17.05MB / 17.05MB                                                                                                                                        15.1s 
 => => sha256:4fb255c76461d67abc91f408697fa9ebae85009a5d6d94a905e52946bab9baa0 176B / 176B                                                                                                                                              15.3s 
 => => extracting sha256:1bc677758ad7fa4503417ae5be18809c5a8679b5b36fcd1464d5a8e41cb13305                                                                                                                                                0.5s 
 => => extracting sha256:0d0e0ecb256ae3e8b7625494ed35189f845766552b7159a17f634706e28a9687                                                                                                                                                0.4s 
 => => extracting sha256:c24bf4c725c221ddbb66e056716cf961f79001fedf2e8a0cddab347244398bac                                                                                                                                                1.4s 
 => => extracting sha256:4fb255c76461d67abc91f408697fa9ebae85009a5d6d94a905e52946bab9baa0                                                                                                                                                0.0s 
 => [internal] load build context                                                                                                                                                                                                        0.0s 
 => => transferring context: 1.25MB                                                                                                                                                                                                      0.0s 
 => [2/6] WORKDIR /app                                                                                                                                                                                                                   0.3s 
 => [3/6] COPY .mvn/ .mvn                                                                                                                                                                                                                0.0s 
 => [4/6] COPY mvnw pom.xml ./                                                                                                                                                                                                           0.0s 
 => [5/6] RUN ./mvnw dependency:resolve                                                                                                                                                                                                263.0s 
 => [6/6] COPY src ./src                                                                                                                                                                                                                 0.0s 
 => exporting to image                                                                                                                                                                                                                   0.7s 
 => => exporting layers                                                                                                                                                                                                                  0.7s 
 => => writing image sha256:5826c003b44c53f42c165e3d434953d68e7aff6d20eed5e54ddd3af3a8a1c7dc                                                                                                                                             0.0s 
 => => naming to docker.io/library/java-spring                                                                                                                                                                                           0.0s 
```

### View local images

* [x] Using the "docker images" command, observe your images, and the associates tag.

### Result expected:

```
docker images
REPOSITORY        TAG            IMAGE ID       CREATED          SIZE
<yourRepository>  <yourTag>      910e5f0c8b1f   11 minutes ago   606MB
eclipse-temurin   17-jdk-jammy   56c7bc12ee6d   3 days ago       456MB
```

```
[INPUT]
docker images

[OUTPUT]
REPOSITORY    TAG       IMAGE ID       CREATED         SIZE
java-spring   dev    5826c003b44c   2 minutes ago   606MB
```

## Using tags

* [ ] Using the appropriate command, try to obtain this situation on your local machine:

```
[INPUT]
docker images

[OUTPUT]
REPOSITORY        TAG            IMAGE ID       CREATED       SIZE
petclinic         dev            323bdb488603   2 hours ago   606MB
petclinic         int            323bdb488603   2 hours ago   606MB
petclinic         prod           323bdb488603   2 hours ago   606MB
eclipse-temurin   17-jdk-jammy   56c7bc12ee6d   9 days ago    456MB
```

* [ ] Is it a good idea to use tags like this to create different stages (dev, int, prod) ?

> The Docker tag helps maintain the build version to push the image to the Docker Hub**. The Docker Hub allows us to group images together based on name and tag.** Multiple Docker tags can point to a particular image. Basically, as in Git, Docker tags are similar to a specific commit. Docker tags are just an alias for an image ID.
>
> The tag's name must be an ASCII character string and may include lowercase and uppercase letters, digits, underscores, periods, and dashes. In addition, the tag names must not begin with a period or a dash, and they can only contain 128 characters.
>
> Source : [baeldung.com](https://www.baeldung.com/ops/docker-tag)

```
//TODO
Explain
```

* [ ] Using the appropriate command, update your local images and tags like this:

```
[INPUT]
docker images

[OUTPUT]
REPOSITORY          TAG              IMAGE ID       CREATED        SIZE
eclipse-petclinic   version1.0.dev   323bdb488603   18 hours ago   606MB
eclipse-temurin     17-jdk-jammy     56c7bc12ee6d   10 days ago    456MB
```

```
[INPUT]
//TODO

[OUTPUT]
//TODO
```

