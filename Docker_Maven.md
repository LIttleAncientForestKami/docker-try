# Docker and Maven - how to?

## 1. Dockerfile in Maven project
Usual Maven business. Add a Dockerfile:

vim Dockerfile
i
FROM java:8
EXPOSE 8080
ADD /target/demo.jar demo.jar
ENTRYPOINT ["java", "-jar", "demo.jar"]
Esc
:wq

Now, clean, install, then docker build:

`docker build -t demo-app .`
`docker run -p 8080:8080 -t demo-app`

## 2. Via Maven plugin
Spotify made one.

```
<plugin>
    <groupId>com.spotify</groupId>
    <artifactId>docker-maven-plugin</artifactId>
    <version>0.4.5</version>
        <configuration>
            <imageName>springdocker</imageName>
            <baseImage>java</baseImage>
            <entryPoint>["java", "-jar", "/${project.build.finalName}.jar"]</entryPoint>
            <resources>
                <resource>
                    <targetPath>/</targetPath>
                    <directory>${project.build.directory}</directory>
                    <include>${project.build.finalName}.jar</include>
                </resource>
            </resources>
        </configuration>
</plugin>

```
Now build and run:

```
mvn clean package docker:build
docker images
docker run -p 8080:8080 -t <image name>
```

## 3. Like Alooma described

https://www.alooma.com/blog/building-dockers

## 4. Maven in a container

Have a base box.
Add Maven.
Put source dir there, run from there.


```
docker run -it --rm \
       -v "$(pwd)":/opt/maven \
       -w /opt/maven \
       maven:latest \
       mvn clean install
```

Feel free to also use Nexus there.
