# CONTINUOUS DELIBERY (CD) ♾️
### Maven example "hello world"
Using:
- Spring
- Github
- Docker
- Maven


Go to [start.spring.io](https://start.spring.io/) for create a example "hello world" app

![spring.io](img/spring.io.png)

When done click "Generate" button, this will download the project, extract it into a folder.


Go into the directory of extracted project and see the contents:
```bash
ls -a
# .DS_Store
# .git
# .gitignore
# .mvn
# HELP.md
# mvnw
# mvnw.cmd
# pom.xml
# src
```

Init the repository and upload it to github (generate it first on github):
```bash
git init
git add .
git commit -m "add files generated with start.spring.io, maven example project"
git remote add origin git@github.com:iranzoferri/continuous-delivery-example.git
```

Ok, now we go to build the application using docker, manually.

```bash
absolute_path=/home/... # <-- Put here the project path

# Create a volume for persistence:
docker volume create m2

# Run maven container:
docker container run --rm -it -v ${absolute_path}/continuous-delivery-example:/app maven:alpine sh
```

This provide us a running container with an interactive shell for typing next command:

```bash
mvn spring-boot:run -f app/pom.xml
```

This is the result (You will see a lot of lines)
![build_success](img/build_success.png)

Inside the container:
```bash
ls /root/.m2/
# repository
# copy_reference_file.log
# settings-docker.xml
```

Then, now we can simplify the build process with docker with persistence, using this command:

```bash
docker container run --rm -it -v m2:/root/.m2 -v /home/jaime/repos/github/continuous-delivery-example:/app maven:alpine mvn spring-boot:run -f app/pom.xml
```