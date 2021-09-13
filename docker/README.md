# Quick Start

## First container

### Commands

1. Check docker version: `docker --version`
2. Check docker status: `docker ps`
3. Navigate to <https://hub.docker.com/repository> and create a repository
4. Navigate to first container folder: `cd gsd/first-container`
5. Build local image: `docker image build -t echogame/playground:echo-ctr-v1 .`
6. List images: `docker image ls`
7. Login into docker with username and pwd: `docker login -u <username> -p "<password>"`
8. Push image to the registry: `docker image push echogame/playground:echo-ctr-v1`
9. Remove local image: `docker image rm echogame/playground:echo-ctr-v1`
10. Run uploaded image: `docker container run -d --name web -p 8000:8080 echogame/playground:echo-ctr-v1`
11. List containers: `docker container ls`
12. Browse the running app: <http://localhost:8000/>
13. Stop container using the name `docker container stop web` or the id `docker container stop 92d0c677399d`
14. List stopped containers: `docker container ls -a`
15. Start the container again using the name `docker container start web`
16. Delete the container
    - Stop if it's running `docker container stop web`
    - And delete `docker container rm web`

### Run container interactivelly

1. `docker container run -it --name test alpine sh`
    - `-it` = interactive
    - `--name test`
    - `alpine` = base image
    - `sh` = command to run at the start
2. Exit interactive mode without killing the container: `Ctrl + P + Q`
3. List containers:

    ```bash
    > docker container ls
    CONTAINER ID   IMAGE     COMMAND   CREATED          STATUS          PORTS     NAMES
    d1b9ec598d2e   alpine    "sh"      51 seconds ago   Up 49 seconds             test
    ```

4. Force remove a running container: `docker container rm test -f`
