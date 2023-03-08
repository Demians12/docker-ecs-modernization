# docker-ecs-modernization

- db a mysql 8 to use directly from Docker hub for persistent volume. for the first module it uses the secret located in a file db/password.txt. This secret is used by the backend to connect to mysql database.
- backend is a python application API.
- frontend is just a proxy served up by Nginx to listen traffic on port 80. It is a Flask app.


## Running first step
### run in the root folder:
- `docker compose build`
- `docker images` to see the created images
`REPOSITORY                          TAG       IMAGE ID       CREATED             SIZE
docker-ecs-modernization-frontend   latest    127ad9daae74   About an hour ago   18MB
docker-ecs-modernization-backend    latest    99039f8e96b5   About an hour ago   74MB` 


### Push images to Docker Hub
- `docker login -u docker_hub_user`
- `docker tag docker-compose-ecs-backend:latest ${DOCKER_HUB_ID}/docker-compose-ecs-backend:latest`
- `docker tag docker-compose-ecs-frontend:latest ${DOCKER_HUB_ID}/docker-compose-ecs-frontend:latest`

- `docker push ${DOCKER_HUB_ID}/docker-compose-ecs-backend:latest`
- `docker push ${DOCKER_HUB_ID}/docker-compose-ecs-frontend:latest`