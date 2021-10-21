## Pre-reqs
1. kubectl
2. eksctl
3. docker

## Clone the kimai repo containing Dockerfile and other dependencies and build image
1. git clone https://github.com/tobybatch/kimai2.git
2. cd kimai2
3. docker build -t kimai .
4. docker run -d -p 8001:8001 --name kimai node-docker (to test if the image works)

## Create ECR to store locally built image
aws ecr create-repository --reposritory-name kimai --region us-east-1

## Authenticate docker to ECR
1. aws ecr get-login-password --region us-east-1
2. aws ecr --region us-east-1|docker login -u AWS -p <token> 694397500817.dkr.ecr.us-east-1.amazonaws.com/kimai

**## Tag docker image**
docker tag kimai:latest 694397500817.dkr.ecr.us-east-1.amazonaws.com/kimai:latest

## Push local image to ECR
docker push 694397500817.dkr.ecr.us-east-1.amazonaws.com/kimai:latest

## Create K8s manifest files to use pushed image
1. Deploy
2. Service
3. Ingress




