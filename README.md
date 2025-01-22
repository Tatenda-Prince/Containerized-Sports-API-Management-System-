# Containerized-Sports-API-Management-System-
"Game Day Schedule API"

# Technical Architecture

![image_alt](https://github.com/Tatenda-Prince/Containerized-Sports-API-Management-System-/blob/9e9c5d51f1671d820b08ec2422242be2e0b0f5ce/images/Screenshot%202025-01-22%20115532.png)

## Project Overview

This project explains how to create a containerised API management system for accessing sports data. It uses Amazon ECS (Fargate) to run containers, Amazon API Gateway to expose REST endpoints, and an external Sports API to provide real-time sports data. The project demonstrates advanced cloud computing methods such as API administration, container orchestration, and secure AWS connections.

## Project Objectives

1.Exposes a REST API for querying real-time sports data

2.Runs a containerized backend using Amazon ECS with Fargate

3.Scalable and serverless architecture

4.API management and routing using Amazon API Gateway 

## Prerequisites

1.Sports API Key: Sign up for a free account and subscription & obtain your API Key at serpapi.com

2.AWS Account: Create an AWS Account & have basic understanding of ECS, API Gateway, Docker & Python

3.AWS CLI Installed and Configured: Install & configure AWS CLI to programatically interact with AWS

4.Serpapi Library: Install Serpapi library in local environment "pip install google-search-results"

5.Docker CLI and Desktop Installed: To build & push container images

## Technologies

1.Cloud Provider: AWS

2.Core Services: Amazon ECS (Fargate), API Gateway, CloudWatch

3.Programming Language: Python 3.x

4.Containerization: Docker

5.IAM Security: Custom least privilege policies for ECS task execution and API Gateway

## Use case



## Project Structure

![image_alt](https://github.com/Tatenda-Prince/Containerized-Sports-API-Management-System-/blob/3c988bc311d268e1e1748349be8b6c01be2d40a6/images/Screenshot%202025-01-22%20121016.png)



## Setup Instructions


## Clone the Repository

```language
https://github.com/Tatenda-Prince/Containerized-Sports-API-Management-System-.git

cd containerized-sports-api
```

![image_alt](https://github.com/Tatenda-Prince/Containerized-Sports-API-Management-System-/blob/a304cdc34895b1a5f36d3c8b3e02fa5002f893f0/images/Screenshot%202025-01-22%20122714.png)


## Create ECR Repo

```language
aws ecr create-repository --repository-name sports-api --region us-east-1
```

![image_alt](https://github.com/Tatenda-Prince/Containerized-Sports-API-Management-System-/blob/8b185e5063a6846b6d11e9e74540b8f526475f48/images/Screenshot%202025-01-22%20122753.png)


Navigate to aws console and you wil see that AWS ECR was created as shown below:


![image_alt](https://github.com/Tatenda-Prince/Containerized-Sports-API-Management-System-/blob/00f860708c45aa0279f3e56dc39454516da89b0b/images/Screenshot%202025-01-22%20122829.png)



## Authenticate Build and Push the Docker Image

Now are going to get authorization which allows to access our AWS ECR in AWS

```language

aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com
```

![image_alt](https://github.com/Tatenda-Prince/Containerized-Sports-API-Management-System-/blob/6a3364f610ce5f4cfef3af5bea70f90140f7f46f/images/Screenshot%202025-01-22%20123222.png)

Next we're going to build our image, tag it and push it to aws in our ECR repository by copying and paste this commands

```language

docker build --platform linux/amd64 -t sports-api .

docker tag sports-api:latest <AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/sports-api:sports-api-latest

docker push <AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/sports-api:sports-api-latest

```
![image_alt](https://github.com/Tatenda-Prince/Containerized-Sports-API-Management-System-/blob/ac5ba60b17dff8177c105ad0011742babed34fef/images/Screenshot%202025-01-22%20134127.png)

## Success

our sports api image with latest tags  was successfully pushed into AWS ECR repository as shown below:

![image_alt](https://github.com/Tatenda-Prince/Containerized-Sports-API-Management-System-/blob/f269d2e90c29aca959d94cc8fb94af7fb155da4b/images/Screenshot%202025-01-22%20123410.png)

Now that our image is aws we can go ahead in the console to create a ESC Cluster to run our application. 

## Set Up ECS Cluster with Fargate

We are going to create our ESC cluster using Fargate which is serverless. 

## Create an ECS Cluster:

1.Go to the ECS Console → Clusters → Create Cluster

![image_alt](https://github.com/Tatenda-Prince/Containerized-Sports-API-Management-System-/blob/f0cd1bc2cdd49cf8464c06d20ba4a8826515c81b/images/Screenshot%202025-01-22%20123728.png)


3.Name your Cluster (sports-api-cluster)

![image_alt](https://github.com/Tatenda-Prince/Containerized-Sports-API-Management-System-/blob/ce01acad392a168e0804dc6b94d2f08bd19472e0/images/Screenshot%202025-01-22%20123800.png)


4.For Infrastructure, select Fargate, then create Cluster

![image_alt](https://github.com/Tatenda-Prince/Containerized-Sports-API-Management-System-/blob/f7b1182f36b8c9e20d3ce70bddcc7d78af9f2313/images/Screenshot%202025-01-22%20123955.png)

## Create a Task Definition:

1.Go to Task Definitions → Create New Task Definition

![image_alt](https://github.com/Tatenda-Prince/Containerized-Sports-API-Management-System-/blob/27c89368687b1a34df94649f03b0596c153910d6/images/Screenshot%202025-01-22%20124057.png)

2.Name your task definition (sports-api-task)
For Infrastructure, select Fargate

![image_alt](https://github.com/Tatenda-Prince/Containerized-Sports-API-Management-System-/blob/c08faffdaabdd99e69b5f8233cd6303d12fe873d/images/Screenshot%202025-01-22%20124201.png)


3.Add the container:

Name your container (sports-api-container)
Image URI: <AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/sports-api:sports-api-latest
Container Port: 8080
Protocol: TCP
Port Name: Leave Blank
App Protocol: HTTP

![image_alt](https://github.com/Tatenda-Prince/Containerized-Sports-API-Management-System-/blob/495878b2aec76e96b1e95c5b6855836c37fa1c7b/images/Screenshot%202025-01-22%20124425.png)


4.Define Environment Eariables:
Key: SPORTS_API_KEY
Value: <YOUR_SPORTSDATA.IO_API_KEY>

![image_alt](https://github.com/Tatenda-Prince/Containerized-Sports-API-Management-System-/blob/2f5d54a38c4c4f17509b667302398cb1e8fe27ad/images/Screenshot%202025-01-22%20124518.png)

5.Create task definition

![image_alt](https://github.com/Tatenda-Prince/Containerized-Sports-API-Management-System-/blob/9daccee315a26ac023097b7f9af2343572aa9f8d/images/Screenshot%202025-01-22%20124607.png)


## Run the Service with an ALB

1.Go to Clusters → Select Cluster → Service → Create.

![image_alt](https://github.com/Tatenda-Prince/Containerized-Sports-API-Management-System-/blob/7c6ac92a3174b1e3eec257fa913a207dd06965e5/images/Screenshot%202025-01-22%20124818.png)

2.Capacity provider: Fargate
Note: leave everything as default from the top until you reach the Task Definition option.


3.Select Deployment configuration family (sports-api-task)
Name your service (sports-api-service)
Desired tasks: 2

![image_alt](https://github.com/Tatenda-Prince/Containerized-Sports-API-Management-System-/blob/c37739872d0b6f2eb4455baff8625518b6ff67e7/images/Screenshot%202025-01-22%20125011.png)


4.Networking: Create new security group
Networking Configuration:
Type: All TCP
Source: Anywhere

![image_alt](https://github.com/Tatenda-Prince/Containerized-Sports-API-Management-System-/blob/40df5dd228548082fdb4612eb19d1ff992e462b2/images/Screenshot%202025-01-22%20125101.png)

5.Load Balancing: Select Application Load Balancer (ALB).
ALB Configuration:
Create a new ALB:
Name: sports-api-alb

![image_alt](https://github.com/Tatenda-Prince/Containerized-Sports-API-Management-System-/blob/58794c1da98db49d27e60c3e61fc2a0627fae7a7/images/Screenshot%202025-01-22%20125133.png)


Target Group health check path: "/sports"

![image_alt](https://github.com/Tatenda-Prince/Containerized-Sports-API-Management-System-/blob/9fdeab66b9b2d883ee78898744456a85b08cc5f1/images/Screenshot%202025-01-22%20125159.png)

then proceed to click on create Service 


## Test the ALB:















