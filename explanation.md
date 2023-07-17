->Choice of the base image on which to build each container.
-->Client side container- Node image as this is a node.js application. The node image is based on alpine; a minimalistic linux variant, to keep the image size small.
-->Backend side container- Node image as this is a node.js application. The node image is based on alpine; a minimalistic linux variant, to keep the image size small.

->Dockerfile directives used in the creation and running of each container.
-->Client side Dockerfile:

#Sets the base image as Node which is built on top of alpine, a lightweight linux distribution.

FROM node:14-alpine

#Sets the workdirectory to app/client and the subsequent instructions will be executed relative to this path.

WORKDIR /app/client

#Copy the package.json and package-lock.json from the build context where the Dockerfile is located to the current #working directory.

COPY package\*.json ./

#Install dependencies listed in package.json.

RUN npm install

#Copies all the files in the build context to the current working directory in the container.

COPY . .

#Informs that the container will listen on port 3000

EXPOSE 3000

#Command to start the container

CMD ["npm", "run", "start"]

-->Backend side Dockerfile:

#Sets the base image as Node which is built on top of alpine, a lightweight linux distribution.

FROM node:14-alpine

#Sets the workdirectory to app/client and the subsequent instructions will be executed relative to this path.

WORKDIR /app/backend

#Copy the package.json and package-lock.json from the build context where the Dockerfile is located to the current #working directory.

COPY package\*.json ./

#Install dependencies listed in package.json.

RUN npm install

#Copies all the files in the build context to the current working directory in the container.

COPY . .

#Informs that the container will listen on port 5000

EXPOSE 5000

#Command to start the container

CMD ["npm", "run", "start"]

->Docker-compose Networking (Application port allocation and a bridge network implementation) where necessary.

The services(client, backend and mongo) in the docker-compose.yml by default create a bridge network and are able to communicate to each other using their service names as DNS hostnames.
In the client service, port 3000 in the client container is mapped to port 3000 of the host.While in the backend service, port 5000 in the backend container is mapped to port 5000 of the host. This allows access to services running in the containers via the specified ports on the host machine.

->Docker-compose volume definition and usage (where necessary).
The volume defined; mongodb_data is used to persist MongoDB data by mounting it to the /data/db directory inside the mongo container.
The volume allows the MongoDB data to be shared outside the container.

->Git workflow used to achieve the task.
Forked repository
Cloned repository in local machine
Edited the application and pushed commits to the remote repository.

->Successful running of the applications and if not, debugging measures applied.
Added .env file that was missing with the MONGODB_URI

->Good practices such as Docker image tag naming standards for ease of identification of images and containers.
-->Image tagging and version for both the client side and backend side in the docker-compose.yml file
Used semantic versioning in the docker-compose.yml file for easy identification of images built
image: client-side-image:1.0.0
image: backend-side-image:1.0.0
