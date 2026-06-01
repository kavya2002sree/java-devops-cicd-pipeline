# 🚀 Java-devops-cicd-pipeline

## 📌 Project Description
A complete DevOps project showcasing CI/CD automation with Jenkins, containerization using Docker, image distribution via Docker Hub, Kubernetes-based deployment, and application monitoring using Grafana/Splunk

## 🏗️ Architecture Overview

Workflow Explanation
1. Source code is stored in GitHub
2. Jenkins pulls the code and triggers the pipeline
3. Maven builds the Java application (.war file)
4. Docker builds an image using the Dockerfile

## 🛠️ Tech Stack

  * Java
  * Maven
  * Jenkins
  * Docker
  * Docker Hub
  * Kubernetes
  * AWS EC2
  * Grafana / Splunk

## ⚙️ Prerequisites

* AWS EC2 instance (Linux)  
* Docker installed  
* Jenkins installed  
* Java (JDK 21 or above)  
* Maven installed  
* Kubernetes cluster (Minikube / EKS)  
* Docker Hub account  
* GitHub repository

## ⚙️ Prerequisites

* AWS EC2 Instances
* Java JDK 21+
* Maven
* Jenkins
* Docker
* Git
* GitHub Repository
* SSH Key Pair
  
## 🚀 Features

* Automated CI/CD Pipeline
* Source Code Management using GitHub
* Automated Maven Builds
* WAR Artifact Generation
* Docker Image Creation
* Containerized Deployment
* Jenkins Build Automation
* Remote Deployment via SSH
* Continuous Application Delivery
  
## 📂 Pipeline Stages

 Stage 1: Source Code Management
 Store application source code in GitHub.
 
 Stage 2: Continuous Integration
 Jenkins pulls code automatically.
 Maven builds and packages the application.
 
 Stage 3: Artifact Management
 WAR file generated inside the target directory.
 
 Stage 4: Containerization
 Docker image created using Dockerfile.
 WAR file deployed to Tomcat container.
 
 Stage 5: Deployment
 Container launched on Docker Host.
 Application exposed through public IP and port.

## 🌐 Application Deployment Flow

Developer → GitHub → Jenkins → Maven Build → WAR File → Docker Image → Docker Container → End User

## 🔮 Future Enhancements

* Kubernetes Deployment
* Terraform Infrastructure Provisioning
* Docker Hub Image Repository
* AWS EKS Integration
* Grafana Monitoring Dashboard
* Splunk Log Aggregation
* Prometheus Metrics Collection
* Automated Rollbacks
* Blue-Green Deployment Strategy
