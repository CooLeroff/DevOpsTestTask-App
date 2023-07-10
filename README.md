# Exquance DevOps test task

[![DevOps_App_Deploy](https://github.com/CooLeroff/DevOps_test_task/actions/workflows/docker-image.yml/badge.svg)](https://github.com/CooLeroff/DevOps_test_task/actions/workflows/docker-image.yml)

## Application description

"Goals" application consist of 2 parts: backend (Node.js app) and frontend (React app). Applications interacts with each other via REST API. Backend also connects to MongoDB to store data (user goals).

User can add goals and delete them.

## Technical details

Apps have dockerfiles to build images for them.

To make frontend app start and work correctly you need to run container with `-it` flags.

## Backend app

Environment variables to configure app:

- MONGODB_USERNAME - username to connect to MongoDB.
- MONGODB_PASSWORD - password to connect to MongoDB.
- MONGODB_URL - url of MongoDB server.
- MONGODB_PORT - port of MongoDB server.

## Frontend app

Environment variables to configure app:

- REACT_APP_BACKEND_URL - url of backend REST API.

## Local run

Use ```docker compose``` for build and run the application locally.

Make sure you have docker and docker compose installed to run, otherwise see the installation documentation.: https://docs.docker.com/engine/install/

Opent terminal in root of project and run
```docker compose up --profile local```

## CI Deploy