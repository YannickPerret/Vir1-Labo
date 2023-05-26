# Step 1 - Run the project outside Docker

* [Official Source](https://docs.docker.com/language/java/build-images/)

## Get the project source code

* Clone the repo

```
git clone https://github.com/spring-projects/spring-petclinic.git
```

* Read the readme file carefully

<!---->

* [x] What type of application is it ?
    Spring Boot
* [x] Which database engine is used ?
    h2
* [x] Do we need to install the package manager _MAVEN_ before building the project ?
    no

<!---->

* Inspect the dependencies (pom.xml)

<!---->

* [x] Which version of Java should compatible with the code provided ?
    Java 17 or newer

## Setup Java components

### Check your current Java installation

* [x] Where is java installed ?

```
[INPUT]
which java

[OUTPUT]
/c/Program Files (x86)/Common Files/Oracle/Java/javapath/java
```

* [x] Which current compiler is installed (JDK) ?

```
[INPUT]
javac -version

[OUTPUT]
javac 20.0.1upstream/main
```

* [x] Which current runtime is installed (JRE) ?

```
[INPUT]
java -version

[OUTPUT]
java version "20.0.1" 2023-04-18
Java(TM) SE Runtime Environment (build 20.0.1+9-29)
Java HotSpot(TM) 64-Bit Server VM (build 20.0.1+9-29, mixed mode, sharing)
```

* [ ] Do we need to install the Java virtual machine (JVM)?

```
//TODO
```

### Install the Open JDK

* [Oracle Download WebSite](https://jdk.java.net/20/)

{% hint style="info" %}
* Accept the end user license before trying, then
* Then get the target URL (cookies).
{% endhint %}

```powershell
[INPUT]
curl https://download.java.net/java/GA/jdk20.0.1/b4887098932d415489976708ad6d1a4b/9/GPL/openjdk-20.0.1_windows-x64_bin.zip -O openjdk-20.0.1_windows-x64_bin.zip

[OUTPUT]
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  187M  100  187M    0     0  3678k      0  0:00:52  0:00:52 --:--:-- 3740k
curl: (6) Could not resolve host: openjdk-20.0.1_windows-x64_bin.zip
```

#### Check the archive integrity before installing the JDK

* [Get the hash provided by Oracle](https://download.java.net/java/GA/jdk20.0.1/b4887098932d415489976708ad6d1a4b/9/GPL/openjdk-20.0.1\_windows-x64\_bin.zip.sha256)
* Generate your local hash based on the archive downloaded ([help](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/get-filehash?view=powershell-7.3))
* Compare both hashes...

```powershell
[INPUT]
CertUtil -hashfile openjdk-20.0.1_windows-x64_bin.zip SHA256

[OUTPUT]
31ca4a8cbdea1da7fb441194e756dd1adbedfc05bd1135a1ecc46b4288ea8942
```

#### Unzip jdk archive

```
[INPUT]
unzip openjdk-20.0.1_windows-x64_bin.zip

[OUTPUT]
Archive:  openjdk-20.0.1_windows-x64_bin.zip
   creating: jdk-20.0.1/
   creating: jdk-20.0.1/bin/
   ...
```

#### Move the unzip folder to Programs Folder

```
[INPUT]
mv jdk-20.0.1\ "C:\Program Files\Java"

[OUTPUT]

```

#### Set environment variables

* [Java official documentation](https://dev.java/learn/getting-started/)

<!---->

* [x] Set JAVA\_HOME variable

```
[INPUT]
setx /m JAVA_HOME "C:\Program Files\Java\jdk-20.0.1\"

[OUTPUT]
RÉUSSITE : la valeur spécifiée a été enregistrée.
```

* [x] Update PATH environment variable

{% hint style="info" %}
Back up your current path before updating it.

echo %PATH% > path.back
{% endhint %}

```
[INPUT]
Ajouter dans le path via l'interface windows
%JAVA_HOME%\bin

[OUTPUT]
```

* [x] Check the variables settings

```
[INPUT]
java -version

[OUTPUT]
java version "20.0.1" 2023-04-18
Java(TM) SE Runtime Environment (build 20.0.1+9-29)
Java HotSpot(TM) 64-Bit Server VM (build 20.0.1+9-29, mixed mode, sharing)
```

## Build and test the project

```
[INPUT]
.\mvnw.cmd spring-boot:run

[OUTPUT]

[INFO] Attaching agents: []


              |\      _,,,--,,_
             /,`.-'`'   ._  \-;;,_
  _______ __|,4-  ) )_   .;.(__`'-'__     ___ __    _ ___ _______
 |       | '---''(_/._)-'(_\_)   |   |   |   |  |  | |   |       |
 |    _  |    ___|_     _|       |   |   |   |   |_| |   |       | __ _ _
 |   |_| |   |___  |   | |       |   |   |   |       |   |       | \ \ \ \
 |    ___|    ___| |   | |      _|   |___|   |  _    |   |      _|  \ \ \ \
 |   |   |   |___  |   | |     |_|       |   | | |   |   |     |_    ) ) ) )
 |___|   |_______| |___| |_______|_______|___|_|  |__|___|_______|  / / / /
 ==================================================================/_/_/_/

:: Built with Spring Boot :: 3.0.4
```

### Result expected 

```
[INPUT]
curl --request GET --url http://localhost:8080/actuator/health --header 'content-type: application/json'

[OUTPUT]
{"status":"UP"}
```
