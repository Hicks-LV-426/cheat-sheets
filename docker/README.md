# Quick Start

## First container

1. Check docker version: `docker --version`
2. Check docker status: `docker ps`
3. Open <https://hub.docker.com/repository> and create a repository
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

## Run container interactivelly

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

## 2nd Container: Docker Compose

1. Check docker-compose installed: `docker-compose version`
2. Navigate to multi-container folder: `cd gsd/multi-container`
3. Build image using docker compose: `docker-compose up -d`
4. Open <http://localhost:5000/>
5. Stop the application using docker compose: `docker-compose down`

## Swarms

1. Intialize: `docker swarm init --advertise-addr=127.0.0.1`

    ```bash
    Swarm initialized: current node (zajadijabve8txql77uwk42xh) is now a manager.

    To add a worker to this swarm, run the following command:

        docker swarm join --token SWMTKN-1-5jga51ktuvukfjdtf75cb59w451j914i54tmv6ltxt33wkqjce-7ty94mw1tnaimgwultfn913iz 127.0.0.1:2377

    To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.
    ```

2. List all managers in a swarm: `docker node ls`
3. Scaling a docker service: `docker service scale reporting=4`
4. Spin-up 3 containers as a service: `docker service create --name web -p 8080:8080 --replicas 3 echogame/playground:echo-ctr-v1`
5. List the containers on current node: `docker container ls`
6. List all services in a swarm by name 'web': `docker service ps web`
7. Open <http://localhost:8080/>
8. Scale the 'web' service: `docker service scale web=10`

    ```bash
    web scaled to 10
    overall progress: 10 out of 10 tasks
    1/10: running   [==================================================>]
    2/10: running   [==================================================>]
    3/10: running   [==================================================>]
    4/10: running   [==================================================>]
    5/10: running   [==================================================>]
    6/10: running   [==================================================>]
    7/10: running   [==================================================>]
    8/10: running   [==================================================>]
    9/10: running   [==================================================>]
    10/10: running   [==================================================>]
    verify: Service converged
    ```

9. If some containers are manually force-removed (mocking a fail): `docker container rm 563447b53c0d c76c80db9285 -f` the swarm will automatically spin up two new containers to ensure that there are 10 in total.
10. Cleanup
    - Remover 'web' service: `docker service rm web`

## Swarm Stack: Swarm from configuration

1. Navigate to swarm-stack folder: `cd gsd/swarm-stack`
2. Swarm stack will not build an image, so it has to be pre-built, e.g.
    - Build the docker image: `docker image build -t echogame/playground:swarm-stack-v1 .`
    - Push image to docker repo: `docker image push echogame/playground:swarm-stack-v1`
    - Remove image from locak cache: `docker image rm echogame/playground:swarm-stack-v1`
3. Deploy a docker stack: `docker stack deploy -c docker-compose.yml counter`
4. List stacks: `docker stack ls`
5. List stack services: `docker stack services counter`
6. Open <http://localhost:5000/>
7. Cleanup: `docker stack rm counter`
