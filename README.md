# README

Devteds [Episode #6](https://devteds.com/episodes/6-deploy-dockerized-web-api-application-on-amazon-ec2-with-rds)

Single-node deployment of a containerized API application on Amazon EC2 with database on RDS 
(Amazon Relational Database Service) using docker machine and docker compose.

[Episode video link](https://youtu.be/bU9o2fVreRU)

[![Episode Video Link](https://i.ytimg.com/vi/bU9o2fVreRU/hqdefault.jpg)](https://youtu.be/bU9o2fVreRU)

## Tested on

* macOS - 10.12
* Docker - 1.13.0
* Docker compose - 1.10.0
* Docker machine - 0.9.0

## Run this application on local

```
git clone https://github.com/devteds/e6-deploy-on-aws.git deploy-on-aws
cd deploy-on-aws
# edit local.yml for local testing env
docker-compose -f local.yml up
# on a separate terminal window
docker-compose -f local.yml run --rm app rails db:migrate
docker-compose -f local.yml run --rm app rails db:seed
# Use REST client or curl to browse the APIs
curl http://localhost:3007/posts
```

## Deploy on Amazon

Follow the instructions from the video. Below are the highlights,

- Spin up RDS on AWS 
- Edit staging.yml for the database configs and endpoint
- Create a new user using AWS IAM
- Install AWS CLI, if not installed already

```
aws configure
docker-machine create -d amazonec2 --amazonec2-region=<REGION FROM AWS> node1
docker-machine ls
eval $(docker-machine env node1)

# enter the API key id and secret for the user created on IAM
docker-compose -f staging.yml run --rm app rails db:migrate
docker-compose -f staging.yml run --rm app rails db:seed
docker-compose -f staging.yml up -d

# Use REST client or curl to browse the APIs
curl http://$(docker-machine ip node1)/posts

docker-compose -f staging.yml ps
docker-compose -f staging.yml stop
docker-compose -f staging.yml start
docker-compose -f staging.yml logs
```

# Application source code

Source code used in [episode 6](https://devteds.com/episodes/6-deploy-dockerized-web-api-application-on-amazon-ec2-with-rds) 
is from [episode 5](https://devteds.com/episodes/5-deploy-with-docker-single-node-deployment-of-rails-api-application-on-vps)

- Source code: https://github.com/devteds/e5-deploy-with-docker-rails-api-single-node
- Docker image: https://hub.docker.com/r/devteds/rails-api-example/
