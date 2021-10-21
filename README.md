## Create an EC2 instance 
![image](https://user-images.githubusercontent.com/86881823/138300652-815045a5-92af-47c0-9819-f17759bae7a0.png)

## Login via Linux machine
ssh -i deepen.pem ec2-user@<ip>
## Login via Windows machine
## Convert pem to ppk and use putty to login
![image](https://user-images.githubusercontent.com/86881823/138301298-c8dc8c35-0d99-4844-8846-246fb0b804b1.png)

## Save ppk file in auth
![image](https://user-images.githubusercontent.com/86881823/138301704-49a7b79d-d28f-49d6-b49f-42563b26b1df.png)
  
### After login follow below steps -

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
  
## Create R53 hosted zone
![image](https://user-images.githubusercontent.com/86881823/138298752-1bf349a2-d008-4510-99d4-a3d343b1873d.png)

## Add a A record, assuming the IP (1.1.1.1) of nginx controller LB
![image](https://user-images.githubusercontent.com/86881823/138299042-fe3f0ef0-a915-4e38-991e-9ba233ecf508.png)

## Create K8s manifest files to deploy the micro service
1. Deploy
2. Service
3. Ingress
4. HPA
  
## Check the resource 
kubectl get po, deploy, svc, ing, hpa 

## Access the application via browser
web.kimai.com




