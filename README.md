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

![image_alt]()


## Authenticate Build and Push the Docker Image

```language

aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com

docker build --platform linux/amd64 -t sports-api .
docker tag sports-api:latest <AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/sports-api:sports-api-latest
docker push <AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/sports-api:sports-api-latest
```






