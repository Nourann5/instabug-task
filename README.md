# Project Overview

This project aims to deploy a Go application connected to a MySQL database using various tools such as Docker, Jenkins, Kubernetes, and Helm. The deployment process involves creating a Dockerfile to build the application, setting up a Docker Compose file to run the application locally, creating a Jenkins pipeline job to automate the build process and send notifications to a Slack channel, and finally, deploying the application to Kubernetes using Helm manifests.

# How it works
It is exposed on port 9090:
* On `/healthcheck` it returns an OK message, 
* On GET it returns all recorded rows.
* On POST it creates a new row.
* On PATCH it updates the creation date of the row with the same ID as the one specified in query parameter `id`

# Prerequisites
Before proceeding with the deployment of the Go application connected to a MySQL database using Docker, Jenkins, Kubernetes, and Helm, the following prerequisites are set up:

#### 1. Docker: 
Installing Docker on local machine or the target deployment environment. 

#### 2. MySQL Database: 
Setting up a MySQL database . Make sure to adjust the necessary credentials (username, password, host, and port) to connect to the MySQL database.

#### 3. Jenkins: 
Setting up a Jenkins server to automate the build process by creating Jenkins container and install Docker inside it.

#### 4. Kubernetes Cluster: 
Setting up a Kubernetes cluster to deploy the application. used a local Kubernetes cluster Minikube.

#### 5. Helm: 
Installing Helm on local machine or the Jenkins server. Helm is required to package and deploy the application using Helm charts.

## Steps 

### 1. Dockerfile

To build the application, a multistage Dockerfile is used to minimize the image's weight. The Dockerfile should consist of two stages, where the first stage builds the Go application and the second stage copies the built binary into a lightweight base image.


![Screenshot (422)](https://github.com/Nourann5/instabug-task/assets/110049234/41a677e3-b94b-4d7b-8a8e-cab2fde9c588)


### 2. Docker Compose

A Docker Compose file is created to run the application locally and test it with a MySQL database. The file include the necessary configurations and environment variables to ensure proper connectivity between the application and the database.


![Screenshot (410)](https://github.com/Nourann5/instabug-task/assets/110049234/a903b49f-c219-4a75-ab1d-b40f0bfe9fa4)


### 3. Jenkins pipeline job

To automate the build process, a Jenkins pipeline job is created. This job will use the Dockerfile to build the application and add an additional alert to report the success or failure of the build. Notifications will be sent to a Slack channel using the configured Slack plugin in Jenkins.


![Screenshot (419)](https://github.com/Nourann5/instabug-task/assets/110049234/c3793482-0ca6-4abf-a5e4-1b7f2b37730d)


### 4. Configuring Slack channel 

Adjusting Jenkins plugin to establish a connection with the Slack channel. This will enable the pipeline job to send notifications to the designated Slack channel, providing updates on the build status.


![Screenshot (420)](https://github.com/Nourann5/instabug-task/assets/110049234/7e1064ee-92ad-4539-b42f-8d93c32ddee6)


### 5. Helm manifests

Helm manifests is created to deploy the application to Kubernetes. The deployment manifest will also include connecting the application to the MySQL database deployment which is deployed using `kubectl apply -f db.yml` in `Kubernetes/` directory . To deploy the application using helm , execute the command `helm install instabug ./helm` . The Helm manifests also include the following templates:


`ingress.yml`: A template to expose the application to the public.

`hp.yml`: A template to enable horizontal scaling when required.

which is used to expose the app to the public and also confirm auto scaling if enabled 

![Screenshot (424)](https://github.com/Nourann5/instabug-task/assets/110049234/998ad953-c1ec-4d4b-8b65-febf3bae668a)

![Screenshot (416)](https://github.com/Nourann5/instabug-task/assets/110049234/adce2e1d-7556-4512-8cdd-f3c4637db189)



## This README.md file serves as a guide to help understanding the project overview and the necessary guidelines involved in the deployment process.
