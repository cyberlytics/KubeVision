# KubeVision
![alt text](https://github.com/AlbertHahn/KubeVision/blob/main/KubeVision.png)
Logo created in Blender by Albert Hahn.

KubeVision is a microservice web-application, developed for my bachelor thesis.
The application provides a GUI and allows the user to authenticate through either password and username or face-detection.


# Online Kubernetes Deployment with Helm
## Requirements
The local system needs access through kubectl and helm to a running Kubernetes Cluster.
Commands for applying the kubeconfig, export the config as ENV.

`export KUBECONFIG=~/your/path/to/KubeVision/kubeconfig_poc`

### Certs and Secrets are already on the cluster! If not they can be installed with following commands.
`cd helm/certs`\
`kubectl apply -f issuer.yaml`\
`kubectl apply -f cert.yaml`\
`kubectl apply -f mongodb-secret.yaml`

### Deploying KubeVision through Helm.
`cd helm/kubevision`\
`helm install kubevision .`

### Necessary step, execute into shell with bash.
`kubectl exec --stdin --tty deploy/mongo -- /bin/bash`\

### Run this commands in the deployment pod.
`mongo`\
`use admin`\
`db.createUser({user: 'admin',pwd: 'password',roles: [ { role: 'root', db: 'admin' } ]})`

The background for this step is that, the mongodb deployment somehow doesn't initialize a admin user on start, needed for CRUD-Operations.

# Offline Deployment with Docker-Compose
## Requirements
Docker and Docker-Compose Installation on the local machine.

### Start Services with Docker-Compose
`cd microservices`\
`docker-compose up`

### Makefile
`cd microservices`\
`make [COMMAND]`\
e.g. `make build-push-facerecognition`

# Build Services locally without docker
Every service directory has a script for exporting enviromental-variables
e.g. flaskStartupFrontend.sh

### Frontend
`cd microservices`\
`cd frontend`\
`source flaskStartupFrontend.sh`\
`flask run --port=8000`

### Facerecognition
`cd microservices`\
`cd facerecognition`\
`source flaskStartupfacerecognition.sh`\
`gunicorn -k geventwebsocket.gunicorn.workers.GeventWebSocketWorker -w 1 -b 0.0.0.0:5000 run:app`

### Authentication
`cd microservices`\
`cd authentication`\
`source flaskStartupAuth.sh`\
`flask run --port=5001`

### MongoDB needs to be started with docker!
`docker run -p 27017:27017 --name mongodb mongo:4.1.7 `\
`docker exec -it mongodb bash`\
`mongo`\
`use admin`\
`db.createUser({user: 'admin',pwd: 'password',roles: [ { role: 'root', db: 'admin' } ]})`

# DockerHub Repositorys
https://hub.docker.com/repository/docker/albird/frontend

https://hub.docker.com/repository/docker/albird/facerecognition

https://hub.docker.com/repository/docker/albird/authentication

## Tech Stack
cyberlytics/KubeVision is built on the following main stack:

- <img width='25' height='25' src='https://img.stackshare.io/service/993/pUBY5pVj.png' alt='Python'/> [Python](https://www.python.org) – Languages
- <img width='25' height='25' src='https://img.stackshare.io/service/1005/O6AczwfV_400x400.png' alt='Golang'/> [Golang](http://golang.org/) – Languages
- <img width='25' height='25' src='https://img.stackshare.io/service/1030/leaf-360x360.png' alt='MongoDB'/> [MongoDB](http://www.mongodb.com/) – Databases
- <img width='25' height='25' src='https://img.stackshare.io/service/1209/javascript.jpeg' alt='JavaScript'/> [JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript) – Languages
- <img width='25' height='25' src='https://img.stackshare.io/service/1772/s9Bm2Iyx_400x400.jpg' alt='gevent'/> [gevent](http://gevent.org) – Web Servers
- <img width='25' height='25' src='https://img.stackshare.io/service/2179/default_332f874a2edb2686f578aa6389313efcea1eec41.png' alt='NumPy'/> [NumPy](http://www.numpy.org/) – Data Science Tools
- <img width='25' height='25' src='https://img.stackshare.io/service/2375/default_1f67b0ca7416a9f52beb655f90b5602d5ef74b75.jpg' alt='Pillow'/> [Pillow](https://python-pillow.github.io/) – Image Processing and Management
- <img width='25' height='25' src='https://img.stackshare.io/service/4631/default_c2062d40130562bdc836c13dbca02d318205a962.png' alt='Shell'/> [Shell](https://en.wikipedia.org/wiki/Shell_script) – Shells
- <img width='25' height='25' src='https://img.stackshare.io/service/9155/unnamed.jpg' alt='Firebase Authentication'/> [Firebase Authentication](https://firebase.google.com/docs/auth/) – User Management and Authentication
- <img width='25' height='25' src='https://img.stackshare.io/service/11593/no-img.png' alt='DB'/> [DB](https://github.com/infostreams/db) – Database Tools
- <img width='25' height='25' src='https://img.stackshare.io/service/586/n4u37v9t_400x400.png' alt='Docker'/> [Docker](https://www.docker.com/) – Virtual Machine Platforms & Containers

Full tech stack [here](/techstack.md)
