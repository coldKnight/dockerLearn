##Docker Installation

`sudo apt-get update`

`sudo apt-get install apt-transport-https ca-certificates -y`

`sudo apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D`

`sudo echo deb https://apt.dockerproject.org/repo ubuntu-xenial main >> /etc/apt/sources.list.d/docker.list`

`sudo apt-get update`

`sudo apt-get install docker-engine`

`sudo systemctl enable docker`

`sudo usermod -aG docker userName [won't have to use sudo now]`

##Run Docker Containers

`docker run rickfast/hello-oreilley`

`docker run -p 4567:4567 -d rickfast/hello-oreilly-http`
  - `curl localhost:4567/`
  
`docker ps`

`docker stop container_id`

---

####Setup account on docker hub to create and distribute docker images

---

`docker run -i -t ubuntu /bin/bash`

`docker run -d rickfast/hello-oreilly-http`

`docker ps -a` {See list of stopped containers}

`docker run -d --name hello_cont1 rickfast/hello-oreilly-http` {Run a named container; can now stop it with the name}

####Running Containerized Long Running Web Applications

* Going to use Redis for DataStore

`docker run -d -P --name redis redis`

* The database is now running. We'll now start a web application and connect it to our DB.
We can link the containers by `docker link` [deprecated], which allows us to find containers by IP

`docker run --link redis -i -t ubuntu /bin/bash`

* Inside conatiner we can run `env` to see the environment variables

`docker run -d --link redis --name web rickfast/oreilly-simple-web-app`

* The above command does not open a web page because we have not mapped the ports [the -p flag]

`docker kill web`

`docker rm web`

`docker run -p 4567:4567 --link redis --name web rickfast/oreilly-simple-web-app`

* `docker port containerName` tells the port the application is available at inside the docker host
    * The -P flag is vary important for applications that run in dynamic muti-tenant environment
    
