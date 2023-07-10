# DevOps test task (CI)

This is a test task repository with using Docker, Docker Compose, build JS frameworks. 

[![DevOps_App_Deploy](https://github.com/CooLeroff/DevOps_test_task/actions/workflows/docker-image.yml/badge.svg)](https://github.com/CooLeroff/DevOps_test_task/actions/workflows/docker-image.yml)

###Tasks:

- [x] Dockerise application to make possible to run it on the server.
- [x] Create CI/CD on Github actions to build and deploy each part of the application (frontend and backend separately). Plus create step to run frontend app tests (command to run tests can be taken from README.md of frontend app).

"Goals" application consist of 2 parts: backend (Node.js app) and frontend (React app). Applications interacts with each other via REST API. Backend also connects to MongoDB to store data (user goals).

User can add goals and delete them.

###Dockerized environment
The application consists of three docker containers:

- MongoDB (For compatibility reasons uses version 4.4.18)
- Backend application 
- Frontend application (with dev run mode (**npm start**) and production mode (**npm build)** )

### Local run

You can use ```docker compose``` for build and run the application locally.

Make sure you have **docker** and **docker compose** app installed on you host to local run, otherwise see the installation documentation: https://docs.docker.com/engine/install/

------

Open terminal in root folder of project and run
```docker compose --profile local up -d```
where:
```-- profile local``` - run local build with ```npm run``` command
```- d``` - start applicatiion in detached mode from console

-----

After start you can open http://localhost:3000/ to use app

----

For stop local aplication use command:
```docker compose down --remove-orphans --volumes```

### CI Deploy

This repository is prepared for use in the CI GitHub Actions environment

The main pipeline file is **/workflows/docker-image.yml**

The build proceeds as follows:

- Build docker image of backend 
- Docker image frontend in **poduction** mode (npm build).
  When building, the value of the **REACT_APP_BACKEND_URL** variable is set to the IP address of the host
- Buld artefacts and nginx.conf file from `frontend` folder copy to webserver container (Nginx) 
- By docker-compose all three imagres run into containers
- At last step, using Docker_local, a separate docker container named `app_test` is started which deploys the application and runs the tests using the Jest framework engine (`npm tests`)

After deploy, you can reach running application by [http://<host_adress>:80/](http://<host_adress>:80/)

## Technical details and Environment variables

Apps have dockerfiles to build images for them.

To make frontend app start and work correctly you need to run container with `-it` flags.

### Backend app

port: **8111**

Environment variables to configure app:

- MONGODB_USERNAME - username to connect to MongoDB.
- MONGODB_PASSWORD - password to connect to MongoDB.
- MONGODB_URL - url of MongoDB server.
- MONGODB_PORT - port of MongoDB server.

### Frontend app

port: **80**

Environment variables to configure app:

- REACT_APP_BACKEND_URL - url of backend REST API.

  In case of a **local run**, the preset value of - is used http://localhost:3000/ 
  In the case of a **product run**, the IP address of host value is set, which is provided from the GitLab Actions pipeline.

  You can change the value of the REACT_APP_BACKEND_URL argument to match a separate launched backend service with a known URL.
