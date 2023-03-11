# docker-ecs-modernization

- db a mysql 8 to use directly from Docker hub for persistent volume. for the first module it uses the secret located in a file db/password.txt. This secret is used by the backend to connect to mysql database.
- backend is a python application API.
- frontend is just a proxy served up by Nginx to listen traffic on port 80. It is a Flask app.


## Running first step
### run in the root folder:
- `docker compose build`
- `docker images` to see the created images 


### Push images to Docker Hub
- `docker login -u docker_hub_user`
- `docker tag docker-compose-ecs-backend:latest ${DOCKER_HUB_ID}/docker-compose-ecs-backend:latest`
- `docker tag docker-compose-ecs-frontend:latest ${DOCKER_HUB_ID}/docker-compose-ecs-frontend:latest`

- `docker push ${DOCKER_HUB_ID}/docker-compose-ecs-backend:latest`
- `docker push ${DOCKER_HUB_ID}/docker-compose-ecs-frontend:latest`

## Run second step

- `docker compose up -d`
- load aplication `curl http://localhost:3000` 
- insert record in database 
- - `curl http://localhost:3000/add/2/name2` - `curl http://localhost:3000/add/3/name3` - `curl http://localhost:3000/add/4/name4`
- `docker compose down`

# Module 2
## Migrate the app to ECS using docker compose

1. Creating a docker contexto for ecs
1.1 list context (commonly default)
`docker context ls`
1.2 create context
`docker context create ecs`
2. create secrets on aws secrets manager
2.2 create a file with your secrets (I know, we'll improve that)
touch docker-pull-creds.json
{
   "username":"YOUR DOCKERHUB USERID",
   "password":"YOUR DOCKERHUB TOKEN or PASSWORD"
}
2.3 create the secret no aws secret manager
`aws secretsmanager create-secret --name mysecretpassword --secret-file /path/to/mysecretpassword.txt`

3. Deploy to Amazon ECS
`docker compose -f docker-compose.yaml -f docker-compose-migration.yaml up`

Below is the infra architecture defining primitives and application structure
![enter image description here](https://docker.awsworkshop.io/images/application-on-aws.png)

