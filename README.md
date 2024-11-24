Please check [PR_annotation](https://github.com/VasylKhytrych/Helm-Jenkins-WP/blob/main/PR_annotaion.md) file.

# Task 6: Application Deployment via Jenkins Pipeline

This repository contains a simple project. A `Dockerfile` is used to build the container, and a Helm Chart for deploying this application. The `Jenkinsfile` automates running tests, GO unit test, building, and deploying the application.

## Pipeline Steps in the Jenkinsfile

### 1 Docker container setup

In this step, all application dependencies are installed, and installation tests are executed.

### 2 App Unit Tests

In this step, we are running GO tests in docker container.

### 3 Application Build

In this step, we are building our app with docker.

### 4 Push Docker Image to ECR

In this step, we are creating ECR creds and pushing app build to ECR.

### 5 Create ECR Secret

In this step, we are creating k3s secret with ECR key.

### 6 Deploy to Kubernetes with Helm

In this step, our app will be deployed in k3s cluster with HELM via Jenkins pipeline.

### Post - Email Notification

After completing all the steps, an email notification will be sent with the build status.