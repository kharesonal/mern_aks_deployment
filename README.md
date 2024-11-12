Deploy a MERN Application using Azure Kubernetes Service (AKS)
This document provides a step-by-step guide for deploying a MERN stack application on Azure Kubernetes Service (AKS). It outlines all necessary steps, from setting up the environment to deploying the application. Additionally, the README includes instructions on how to configure the necessary files, tools, and resources for deployment, along with relevant screenshots and images that will guide you through the process.

Table of Contents
Prerequisites
Setting Up Azure Kubernetes Service (AKS)
Clone the Repository
Configure Kubernetes Resources
Build Docker Images
Deploy to AKS
Access the Application
Screenshots
Conclusion
Prerequisites
Before you begin, ensure you have the following prerequisites:

Azure Account: You must have an Azure account. If you don’t have one, sign up for a free account here.

Azure CLI: Install the Azure CLI for interacting with Azure resources via the command line. Installation guide.

Kubernetes CLI (kubectl): Install kubectl to interact with your AKS cluster. Installation guide.

Docker: Install Docker to build and push Docker images. Installation guide.

Node.js & npm: Ensure that you have Node.js and npm installed for building the MERN stack application locally. Installation guide.

Git: Install Git to clone the repository. Installation guide.

Setting Up Azure Kubernetes Service (AKS)
Follow these steps to create and set up an AKS cluster:

Create Resource Group: In the Azure CLI, run the following command to create a resource group:

bash
Copy code
az group create --name <your-resource-group> --location eastus
Create AKS Cluster: Next, create an AKS cluster in the resource group:

bash
Copy code
az aks create --resource-group <your-resource-group> --name <aks-cluster-name> --node-count 3 --enable-addons monitoring --generate-ssh-keys
Connect to the AKS Cluster: Once the cluster is created, configure kubectl to interact with your AKS cluster:

bash
Copy code
az aks get-credentials --resource-group <your-resource-group> --name <aks-cluster-name>
Verify the connection to the cluster:

bash
Copy code
kubectl get nodes
Clone the Repository
Clone the MERN application repository:

bash
Copy code
git clone https://github.com/UnpredictablePrashant/SampleMERNwithMicroservices.git
Navigate into the project directory:

bash
Copy code
cd SampleMERNwithMicroservices
Configure Kubernetes Resources
Create Dockerfiles for each of the services (Frontend, Backend, and Database). These files are needed for containerizing your application. If they don't already exist, create them based on the following example:

Frontend Dockerfile (in frontend/ folder):

Dockerfile
Copy code
FROM node:16-alpine
WORKDIR /app
COPY . .
RUN npm install
EXPOSE 3000
CMD ["npm", "start"]
Backend Dockerfile (in backend/ folder):

Dockerfile
Copy code
FROM node:16-alpine
WORKDIR /app
COPY . .
RUN npm install
EXPOSE 5000
CMD ["npm", "start"]
Create Kubernetes Deployment YAML files for each service. These files define the resources for deploying your application on AKS.

Example frontend-deployment.yaml:

yaml
Copy code
apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: frontend
  template:
    metadata:
      labels:
        app: frontend
    spec:
      containers:
      - name: frontend
        image: <docker-image-name>
        ports:
        - containerPort: 3000
Example backend-deployment.yaml:

yaml
Copy code
apiVersion: apps/v1
kind: Deployment
metadata:
  name: backend-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: backend
  template:
    metadata:
      labels:
        app: backend
    spec:
      containers:
      - name: backend
        image: <docker-image-name>
        ports:
        - containerPort: 5000
Build Docker Images
Navigate to the root directory of the application and build the Docker images for both the frontend and backend.

Build Frontend Docker Image:

bash
Copy code
docker build -t <your-dockerhub-username>/frontend:latest ./frontend
Build Backend Docker Image:

bash
Copy code
docker build -t <your-dockerhub-username>/backend:latest ./backend
Push the Images to Docker Hub:

Log in to Docker Hub:

bash
Copy code
docker login
Push the frontend and backend images:

bash
Copy code
docker push <your-dockerhub-username>/frontend:latest
docker push <your-dockerhub-username>/backend:latest
Deploy to AKS
Create Kubernetes Deployments:

Deploy your frontend and backend services using the following command:

bash
Copy code
kubectl apply -f frontend-deployment.yaml
kubectl apply -f backend-deployment.yaml
Create Kubernetes Services:

To expose your services to the internet, create services for the frontend and backend.

Example frontend-service.yaml:

yaml
Copy code
apiVersion: v1
kind: Service
metadata:
  name: frontend-service
spec:
  selector:
    app: frontend
  ports:
    - protocol: TCP
      port: 80
      targetPort: 3000
  type: LoadBalancer
Example backend-service.yaml:

yaml
Copy code
apiVersion: v1
kind: Service
metadata:
  name: backend-service
spec:
  selector:
    app: backend
  ports:
    - protocol: TCP
      port: 5000
      targetPort: 5000
  type: LoadBalancer
Apply the services:

bash
Copy code
kubectl apply -f frontend-service.yaml
kubectl apply -f backend-service.yaml
Access the Application
Once the services are deployed, you can access the application using the external IP provided by Azure.

Get External IP:

Run the following command to get the external IP of your frontend service:

bash
Copy code
kubectl get services frontend-service
This will display an external IP under the EXTERNAL-IP column.

Access the Application:

Open a browser and navigate to http://<external-ip> to access the frontend. The backend API should be accessible at http://<external-ip>:5000.

Screenshots
In this section, include relevant screenshots showing the following:

Creating the AKS cluster in Azure: Capture the process of creating the AKS cluster through the Azure portal or CLI.
Deploying the application to AKS: Take screenshots of kubectl commands and the Kubernetes dashboard showing the deployed pods and services.
Accessing the application: Show the external IP and the running application in the browser.
Conclusion
You have successfully deployed a MERN stack application on Azure Kubernetes Service (AKS). This deployment provides a scalable, reliable environment for your application and takes advantage of the powerful features of AKS, such as auto-scaling and monitoring.

Feel free to modify the configurations and expand the deployment as necessary!

Notes:
Ensure you provide appropriate images at the relevant sections where the guide instructs.
Replace placeholder <docker-image-name>, <your-dockerhub-username>, and <aks-cluster-name> with the actual values you use in your deployment process.