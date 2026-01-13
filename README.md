##  DevOps_Project-Todo_List_API
This project is a simple Todo List REST API built to practice DevOps concepts end to end. It demonstrates CI/CD automation, Docker containerization, security scanning, observability, and Kubernetes deployment.

## Features:
- **API REST** : Task management (CRUD) using FastAPI

- **CI/CD** : Task management (CRUD) using FastAPI

- **Containerisation** : Dockerized application

- **Observability** : /metrics endpoint and logging

- **Orchstration** :Kubernetes deployment

## Installation
pip install -r requirements.txt

## Lancement
python -m uvicorn app:app --reload

## Access the endpoints
- **List Tasks**: http://127.0.0.1:8000/tasks
- **Create task**: POST JSON to http://127.0.0.1:8000/tasks
- **Metrics** : http://127.0.0.1:8000/metrics 
- **Health check** : http://127.0.0.1:8000/health
## Docker
 ## Build the image
   docker build -t todo-api:v1

 ## Run the container
docker run -d -p 8080:8000 --name todo-api:v1
 ## docker-compose
 docker-compose up
 ## Check logs
 docker logs todo-api

## kubernetes
 ## Apply the manifests
 kubectl apply -f deployement.yaml 
 kubectl apply -f service.yaml

 ## Access the API
 kubectl port-forward service/todo-api-service 8000:8000