# Jenkins pipeline tutorial
Project is being done by following [this tutorial](https://youtu.be/6YZvp2GwT0A?si=Cm5fUBhewu83FmIN)

## Setting up Docker

#### The dockerfile
is the default dockerfile from the [jenkins manual](https://www.jenkins.io/doc/book/installing/docker/#downloading-and-running-jenkins-in-docker) (plus python3-pip being installed at the 4th row)

#### Building the docker image
```
docker build -t my-jenkins-blueocean .
```
-t specifies the image's name (and tags after a colon)
the . parameter signifies to look for the Dockerfile in the actual folder

#### Setting up the docker network
```
docker network create jenkins
```
cerates a new docker network named *jenkins*

to verify if it exists run `docker network ls`

#### Run the container
windows command:
```
docker run --name jenkins-blueocean --restart=on-failure --detach \
  --network jenkins --env DOCKER_HOST=tcp://docker:2376 \
  --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 \
  --volume jenkins-data:/var/jenkins_home \
  --volume jenkins-docker-certs:/certs/client:ro \
  --publish 8080:8080 --publish 50000:50000 jenkins-blueocean
```
copied exactly from the [jenkins manual](https://www.jenkins.io/doc/book/installing/docker/#downloading-and-running-jenkins-in-docker)

#### Accessing Jenkins
open a browser and go to `localhost:8080`

it will show you a *getting started page* with some instructions, follow them (use `docker exec jenkins-blueocean <command>`)

The next steps are:
- install jenkins with the suggested plugins
- create first admin user
- provide url address (now *localhost:8080* is OK)

## Setting up jenkins via gui
*to be done*
## Jenkins filesystem and workspace
run `docker exec -it jenkins-blueocean bash` 
  -> gives us a shell into the container running jenkins
  -> the same thing will happen upon ssh-ing into the jenkins container

go into the jenkins folder -> specified via the jenkins-data volume (/var/jenkins_home by default)
  - in the workspace directory the jobs appear as folders with the jobs' name
    - in the folder of a job the same files appear as under the job/workspace tab on the GUI 
