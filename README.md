# alpine-appengine-java
AppEngine Java Docker Images built on Google CloudSDK Alpine Linux

### Usage

Start using your devserver: `docker run --rm -it -h localhost -v ~/.m2:/root/.m2 -v $(pwd):/usr/src/app -w /usr/src/app -p 8080:8080 zenika/alpine-appengine-java`

### Default command

```
mvn -Dapp.devserver.host="0.0.0.0" appengine:run
```

The parameter `app.devserver.host` to `0.0.0.0` allow us to ping our devserver from the container. It's an equivalent to `<host>0.0.0.0</host>` in the `configuration` section of our beloved `pom.xml`

### Deploy commands

Start a bash using `docker run --rm -it -h localhost -v ~/.m2:/root/.m2 -v $(pwd):/usr/src/app -v ~/.config/gcloud:/root/.config/gcloud -w /usr/src/app -p 8080:8080 zenika/alpine-appengine-java bash`

We mount `.config/gcloud` to save the credentials.

Then use the following command:
```
gcloud auth login
#copy paste the url in your browser and then paste the token in your bash
gcloud config set project imt-2017-11
gcloud config set app/promote_by_default false
mvn -Dapp.deploy.version=v0 appengine:deploy
```

After this first deployment, you can simply launch another deployment using:
`docker run --rm -it -h localhost -v ~/.m2:/root/.m2 -v $(pwd):/usr/src/app -v /.config/gcloud:/root/.config/gcloud -w /usr/src/app -p 8080:8080 zenika/alpine-appengine-java mvn -Dapp.deploy.version=v1 appengine:deploy`

### Deploy index

Use the following command `deployIndex`:
```
docker run --rm -it -h localhost -v ~/.m2:/root/.m2 -v $(pwd):/usr/src/app -v /.config/gcloud:/root/.config/gcloud -w /usr/src/app -p 8080:8080 zenika/alpine-appengine-java mvn -Dapp.deploy.version=v1 appengine:deployIndex
```

### Deploy queue

Use the following command `deployQueue`:
```
docker run --rm -it -h localhost -v ~/.m2:/root/.m2 -v $(pwd):/usr/src/app -v /.config/gcloud:/root/.config/gcloud -w /usr/src/app -p 8080:8080 zenika/alpine-appengine-java mvn -Dapp.deploy.version=v1 appengine:deployQueue
```

### Java version

```
docker run --rm zenika/alpine-appengine-java java -version
openjdk version "1.8.0_121"
OpenJDK Runtime Environment (IcedTea 3.3.0) (Alpine 8.121.13-r0)
OpenJDK 64-Bit Server VM (build 25.121-b13, mixed mode)
```

### Maven version

```
docker run --rm zenika/alpine-appengine-java mvn -v
Apache Maven 3.5.2 (138edd61fd100ec658bfa2d307c43b76940a5d7d; 2017-10-18T07:58:13Z)
Maven home: /usr/share/maven
Java version: 1.8.0_121, vendor: Oracle Corporation
Java home: /usr/lib/jvm/java-1.8-openjdk/jre
Default locale: en_US, platform encoding: UTF-8
OS name: "linux", version: "4.9.49-moby", arch: "amd64", family: "unix"
```
