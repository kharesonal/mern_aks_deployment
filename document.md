# Deploying a MERN Application using AKS(Azure kubernetes service)
                                  
This document provides a step-by-step guide for deploying a MERN (MongoDB, Express.js, React.js, Node.js) stack application on Azure Kubernetes Service (AKS). It covers all necessary steps, from setting up the environment to accessing the deployed application. Detailed instructions, configurations, and screenshots are provided to help guide you through the process.

## Table of Contents

1. Prerequisites

2. Setting Up Azure Kubernetes Service (AKS)

3. Clone the Repository

4. Build Docker Images

5. Deploy to AKS

6. Access the Application

7. Conclusion

## Prerequisites

Before starting, make sure you have the following set up:

- **Azure Account**: An active Azure account is required. If you don’t have one, create a free account here.
- **Azure CLI**: Install the Azure CLI to manage Azure resources from the command line.
- **Kubernetes CLI (kubectl)**: Install kubectl for managing and interacting with Kubernetes clusters.
- **Docker**: Ensure Docker is installed for building and pushing container images.
- **Node.js and npm**: Install Node.js and npm to build and run the MERN application locally if needed.
- **Git**: Install Git to clone the application repository to your local system.

## Setting Up Azure Kubernetes Service (AKS)

1. **Log in to Azure**:

      `
       az login
      `

2. **Create a Resource Group**:
   
    `
      az group create --name <your-resource-group> --location eastus
    `

4. **Create an AKS Cluster**:
   
    `
     az aks create --resource-group <your-resource-group> --name <aks-cluster-name> --node-count 3 --enable-addons monitoring --generate-ssh-keys --node-vm-size<Node-size>
    `

5. **Connect to the AKS Cluster**:

    `
     az aks get-credentials --resource-group <your-resource-group> --name <aks-cluster-name>
    `
  
6. **Verify the connection to the cluster**:

   `
    kubectl get nodes
   `
   

   ![image](https://github.com/user-attachments/assets/dcd93faa-8008-4319-8c6e-642074d12516)


## Clone the Repository

   ```
   git clone https://github.com/UnpredictablePrashant/SampleMERNwithMicroservices.git
   
   cd SampleMERNwithMicroservices
   ```
## Build Docker Images
 
 Navigate to each service folder and build Docker images for the following components:

 - Hello Service
 - Profile Service
 - Frontend

## Hello service:

```
cd backend/helloService

docker build -t <dockerhub-username>/hello-service .

```
## Profile service:

```
cd ../profileService

docker build -t <dockerhub-username>/profile-service .
```
## Frontend:

```
cd ../../frontend

docker build -t <dockerhub-username>/frontend .
```

## Push images to Docker Hub:

```
docker push <dockerhub-username>/hello-service

docker push <dockerhub-username>/profile-service

docker push <dockerhub-username>/frontend

```

## Deploy to AKS

- Create Kubernetes Deployments:

  Deploy your frontend and backend services using the following command:

  ```
  cd mernAPP
  helm install simple-mern . -n default
  ```

## Access the Application

- Once the services are deployed, you can access the application using the port forwarding.

   - Get service name: 

Run the following command to get the service name of your frontend service:

 `
 kubectl get services
 `

It will display the frontend service, Portforward it and Access the Application:

`
kubectl port-forward svc/<frontend service name> 3000:3000
`

Open a browser and navigate to http://localhost to access the frontend.


![Screenshot 2024-09-26 203116](https://github.com/user-attachments/assets/5af7f457-5ecb-409f-a5ee-f25657e5ca43)

### Conclusion

You have successfully deployed a MERN stack application on Azure Kubernetes Service (AKS). This deployment provides a scalable, reliable environment for your application and takes advantage of the powerful features of AKS, such as auto-scaling and monitoring.

Notes:

- Ensure you provide appropriate images at the relevant sections where the guide instructs.
- Replace placeholder <docker-image-name>, <your-dockerhub-username>, and <aks-cluster-name> with the actual values you use in your deployment process.









