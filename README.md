# FastAPI Kubernetes Deployment

This project outlines the deployment of a FastAPI application backed by a PostgreSQL database on Kubernetes, utilizing NGINX Ingress for routing and Cert-Manager for SSL certificate management.

## Prerequisites

- Kubernetes Cluster
- kubectl configured to interact with your cluster
- Helm 3

## Components

- **FastAPI Application**: A Python web framework for building APIs with Python 3.7+ based on standard Python type hints.
- **PostgreSQL**: An open-source relational database.
- **NGINX Ingress Controller**: An Ingress controller that uses NGINX as a reverse proxy and load balancer.
- **Cert-Manager**: A Kubernetes tool to automate the management and issuance of TLS certificates from various issuing sources.

## Setup Instructions

### Step 1: Install NGINX Ingress Controller

```shell
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
helm install ingress-nginx ingress-nginx/ingress-nginx --namespace ingress-nginx
```

### Step 2: Install Cert-Manager
```shell
helm repo add jetstack https://charts.jetstack.io
helm repo update
helm install cert-manager jetstack/cert-manager --namespace cert-manager --create-namespace --set installCRDs=true
```
### Step 3: Deploy PostgreSQL
You can use the Bitnami PostgreSQL Helm chart or deploy it manually. Also change the values.yaml file.

```shell
helm repo add bitnami https://charts.bitnami.com/bitnami
helm install db ./helm/db 
```
### Step 4: Deploy FastAPI Application with ingress and clusterIssuer
Create a Docker image for your FastAPI application and push it to a container registry. Use the provided Kubernetes manifests or Helm chart to deploy your application. Also change the values.yaml file.

```shell
helm install fastapi ./helm/fastapi
```
### Accessing the Application
After deployment, access your FastAPI application through the domain specified in your Ingress resource. Ensure DNS records are correctly pointing to your Ingress Controller's external IP or hostname.

### Cleanup
To remove all deployed resources, delete the Helm releases and Kubernetes resources.

```shell
helm uninstall ingress-nginx
helm uninstall cert-manager
helm uninstall db
helm uninstall fastapi
```
