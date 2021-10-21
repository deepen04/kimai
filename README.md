## Install below binaries as pre-reqs
1. kubectl
2. eksctl
3. docker
4. awscli
5. helm

## Create EKS cluster
eksctl create cluster --name kimai-app --version 1.18.0 --region us-east-1 --node-group-name kimai-app-node --node-type t2.micro --node 2

## Check node status
kubectl get nodes

## Create namespace for Nginx Controller components
kubectl create namespace kube-system

## Create Nginx Ingress Controller
1. helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
2. helm repo update
3. helm install nginx-ingress ingress-nginx/ingress-nginx --set controller.publishService.enabled=true -n kube-system

## Check the resoures created
kubectl get all -n kube-system

## Clone the kimai repo containing Dockerfile and other dependencies and build image
1. git clone https://github.com/tobybatch/kimai2.git
2. cd kimai2
3. docker build -t kimai .
4. docker run -d -p 8001:8001 --name kimai kimai (to test if the image works)

## Create ECR to store local image to remote
aws ecr create-repository --reposritory-name kimai --region us-east-1

## Authenticate docker to ECR
1. aws ecr get-login-password --region us-east-1
2. aws ecr --region us-east-1|docker login -u AWS -p <token> 694397500817.dkr.ecr.us-east-1.amazonaws.com/kimai

## Tag docker image
docker tag kimai:latest 694397500817.dkr.ecr.us-east-1.amazonaws.com/kimai:latest

## Push local image to ECR
docker push 694397500817.dkr.ecr.us-east-1.amazonaws.com/kimai:latest
  
## Create R53 hosted zone and add record
![image](https://user-images.githubusercontent.com/86881823/138298752-1bf349a2-d008-4510-99d4-a3d343b1873d.png)

![image](https://user-images.githubusercontent.com/86881823/138299042-fe3f0ef0-a915-4e38-991e-9ba233ecf508.png)

## Create K8s manifest files to deploy the micro service
1. Deploy
2. Service
3. Ingress
4. HPA




