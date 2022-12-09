This repo is cloned from https://github.com/dockersamples/example-voting-app

### How to run the app

The app is basically a simple voting application check the architecture.png for better understanding

### Running the project with plain docker commands

`cd result`

`docker build . -t voting-app`

`cd ..`

`cd worker`

`docker build . -t worker-app`

`cd ..`

`cd vote`

`docker build . -t voting-app`

`cd ..`

`docker pull redis`

`docker pull postgres:9.4`

`docker images` (you will see in total 5 images)

`docker network create my-network`

`docker run --network=my-network -d --name redis redis`

`docker run --network=my-network -d -e POSTGRES_PASSWORD=postgres -e POSTGRES_USER=postgres --name db postgres:9.4`

`docker run -d --name vote --network=my-network -p 5000:80 voting-app`

`docker run -d --name worker --network=my-network worker-app`

`docker run -d --name result --network=my-network -p 5001:80 result-app`

open localhost:5000 and localhost:5001`

### Running the project with docker compose (version 1)

first we need to downgrade docker-compose to version 1

`docker-compose disable-v2`

make sure after completing this demo upgrade back to version by 

`docker-compose enable-v2`

replace the content of docker-compose.yml with (view in raw and copy, dont copy `)

`
redis:
  image: redis:latest

db:
  image: postgres:9.4
  environment:
    POSTGRES_HOST_AUTH_METHOD: trust

vote:
  image: voting-app
  ports:
    - 5000:80
  links:
    - redis

worker:
  image: worker-app
  links:
    - redis
    - db

result:
  image: result-app
  ports:
    - 5001:80
  links:
    - db
`

`docker-compose up`

`ctrl c to stop`

in order to delete the containers open docker desktop 

expand example-voting-app and delete each container individually

### Running the project with docker compose (version 3)

`docker-compose enable-v2`

replace the content of docker-compose.yml with (view in raw and copy, dont copy `)

`
version: "3"
services:
  redis:
    image: redis:latest

  db:
    image: postgres:9.4
    environment:
      POSTGRES_HOST_AUTH_METHOD: trust

  vote:
    image: voting-app
    ports:
      - 5000:80

  worker:
    image: worker-app

  result:
    image: result-app
    ports:
      - 5001:80
`

`docker-compose up`

`ctrl c to stop`

in order to delete the containers open docker desktop 

expand example-voting-app and delete each container individually
