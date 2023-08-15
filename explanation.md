# IP-4

## Google cloud setup

- created google cloud project
- enabled billing
- enabled APIS: Artifact Registry, Cloud Build, Google Kubernetes Engine
- initialize gcloud

## Project Setup

- Created directory locally
- Git cloned the yolo project
- created repos in Artifact Registry for images
- used cloud build to build and push container images
- created a cluster
- created deployment manifest files to deploy client-side, backend-side into pods
- created service manifest files to forward traffic to specified pods, and specified ports to listen to and forward traffic to.
- used LoadBalancer to expose pods externally, to internet traffic
- Defined a stateful set for MongoDB and maintained one replica (Pod) of this StatefulSet to maintain service availability.
- mounted volumes within the container for persistent storage

## External IP

- http://104.198.187.98:3000/

## Testing

- pasted the external IPs on web browser to test adding product in the web app

# IP-3

## Project Setup

- Created directory locally
- Git cloned the yolo project

## Managed Node Setup

- run - 'vagrant init geerlingguy/ubuntu2004' to create a Vagrantfile to define the configuration of Vagrant environment.
- Edited vagrant file, added 'config.vm.provider :virtualbox do |vb|' to make VirtualBox the provider
- run 'vagrant up' to start a geerlingguy/ubuntu2004 ubuntu Virtual Machine that will be the managed node

## Ansible Components

### Inventory

- created hosts file
- Added server name and server IP and port for Ansible to manage

### ansible.cfg

- specified inventory file path
- added remote user: vagrant
- added private_key_file path

### Playbook

- created a playbook.yml in the root folder
- edited vagrant file by adding 'config.vm.provision "ansible" do |ansible| ansible.playbook= "playbook.yml"' to allow ansible to do provision.
- created four roles: initial-set-up, docker-set-up, client, backend, mongodb
- initial-set-up role: updates apt package cache, installs Docker dependencies, add Docker GPG key, adds Docker repository
- docker-set-up role: installs docker and starts docker daemon
- client role: pulls client image from docker hub, runs it and created a network link between backend container
- backend role: pulls backend image from docker hub and runs it
- mongodb role: creates a docker volume, run mongo container and allow connections from outside
- created variables for port numbers and image names inside roles directory
- created blocks and tags for selective execution 'setup', 'front-end-setup', 'backend-setup', 'db-setup'

- run 'vagrant ssh' to connect to VM
- run 'docker ps' to verify that docker containers are running
- run 'docker volume ls' to verify that docker volume was created

# IP-2

## Choice of the base image on which to build each container.

- Client side container- Node image as this is a node.js application. The node image is based on alpine; a minimalistic linux variant, to keep the image size small.
- Backend side container- Node image as this is a node.js application. The node image is based on alpine; a minimalistic linux variant, to keep the image size small.

## Dockerfile directives used in the creation and running of each container.

### Client side Dockerfile:

- Sets the base image as Node which is built on top of alpine, a lightweight linux distribution.
  FROM node:14-alpine

- Sets the workdirectory to app/client and the subsequent instructions will be executed relative to this path.
  WORKDIR /app/client

- Copy the package.json and package-lock.json from the build context where the Dockerfile is located to the current working directory.
  COPY package\*.json ./

- Install dependencies listed in package.json.
  RUN npm install

- Copies all the files in the build context to the current working directory in the container.
  COPY . .

- Informs that the container will listen on port 3000
  EXPOSE 3000

- Command to start the container
  CMD ["npm", "run", "start"]

### Backend side Dockerfile:

- Sets the base image as Node which is built on top of alpine, a lightweight linux distribution.
  FROM node:14-alpine

- Sets the workdirectory to app/client and the subsequent instructions will be executed relative to this path.
  WORKDIR /app/backend

- Copy the package.json and package-lock.json from the build context where the Dockerfile is located to the current working directory.
  COPY package\*.json ./

- Install dependencies listed in package.json.
  RUN npm install

- Copies all the files in the build context to the current working directory in the container.
  COPY . .

- Informs that the container will listen on port 5000
  EXPOSE 5000

- Command to start the container
  CMD ["npm", "run", "start"]

## Docker-compose Networking (Application port allocation and a bridge network implementation) where necessary.

- The services(client, backend and mongo) in the docker-compose.yml by default create a bridge network and are able to communicate to each other using their service names as DNS hostnames.
- In the client service, port 3000 in the client container is mapped to port 3000 of the host.While in the backend service, port 5000 in the backend container is mapped to port 5000 of the host. This allows access to services running in the containers via the specified ports on the host machine.

## Docker-compose volume definition and usage (where necessary).

- The volume defined; mongodb_data is used to persist MongoDB data by mounting it to the /data/db directory inside the mongo container.
- The volume allows the MongoDB data to be shared outside the container.

## Git workflow used to achieve the task.

- Forked repository
- Cloned repository in local machine
- Edited the application and pushed commits to the remote repository.

## Successful running of the applications and if not, debugging measures applied.

- Added .env file that was missing with the MongoDB Url

## Good practices such as Docker image tag naming standards for ease of identification of images and containers.

- Image tagging and version for both the client side and backend side in the docker-compose.yml file
- Image tagged both the client side and backend side containers.
- Used semantic versioning in the docker-compose.yml file for easy identification of images built
- image: ip2-client-side-image:1.0.0
- image: ip2-backend-side-image:1.0.0

## Client side image pushed to docker hub

https://hub.docker.com/repository/docker/cytlin/ip2-client-side-image/general

## Backend side image pushed to docker hub

https://hub.docker.com/repository/docker/cytlin/ip2-backend-side-image/general
