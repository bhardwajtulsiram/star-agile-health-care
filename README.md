# Star Agile Health Care – CI/CD Pipeline

This guide describes a step-by-step Continuous Integration and Continuous Deployment (CI/CD) process for the Star Agile Health Care Java project using Jenkins, Docker, Ansible, Kubernetes and Terraform for AWS infrastructure automation.

---

## Table of Contents

1. [Project Overview](#project-overview)
2. [Prerequisites](#prerequisites)
3. [Pipeline Workflow](#pipeline-workflow)
4. [Step-by-Step Instructions](#step-by-step-instructions)
5. [Troubleshooting & Tips](#troubleshooting--tips)

---

## Project Overview

This project is a Java application (built with Maven) that is containerized using Docker and deployed on cloud infrastructure (Amazon EC2 using Kubernetes), with all stages automated using a Jenkins pipeline. Infrastructure provisioning is automated using Terraform.

---

## Prerequisites

- **Jenkins** server with Pipeline and required plugins
- **Docker** installed on Jenkins agents & target servers
- **Ansible** installed on Jenkins
- **Terraform** installed locally using VS Code
- **AWS EC2** accessible via SSH (for Ansible deployment)
- **Kubernetes** cluster (using `deployment.yaml`)
- **DockerHub** account for pushing images
- **Jenkins credentials** for DockerHub, Github and SSH (for Ansible)
- **Java 11** (OpenJDK 11)

---

## Pipeline Workflow

1. **Provision Infrastructure with Terraform**  
   Use Terraform scripts to provision AWS EC2 instances (and optionally other resources) for deployment.

2.  **Checkout Source Code**  
   Jenkins pulls the latest code from the GitHub repository.

3. **Build & Test**  
   Maven compiles, tests, and packages the application into a JAR.

4. **Docker Build & Push**  
   Docker image is built using the `Dockerfile` and pushed to DockerHub.

5. **Configuration Management**
   - **Using Ansible:**  
     Ansible playbook installs Docker on target EC2(s) and runs the container.
   
6. **Deployment**
   - **Using Kubernetes:**  
     Deploy using `deployment.yaml` deployment manifest file.

---

## Step-by-Step Instructions

### 1. Provision AWS EC2 with Terraform

- Write Terraform configuration files to define AWS EC2 instances.

### 2. Jenkins Pipeline Configuration

- In Jenkins UI, create a new pipeline job.
- Write Groovy pipeline script (Jenkinsfile syntax) into the pipeline configuration.

### 3. Dockerfile

- Defines a Java 11 application image
- Copies the built JAR to the image and sets entrypoint

### 4. Ansible Playbook

- Installs Docker on target EC2(s)
- Starts Docker service
- Runs the application container, mapping ports as defined

### 5. Kubernetes Deployment

- Use `deployment.yaml` to deploy on a Kubernetes cluster

---

## Troubleshooting & Tips

- **Terraform Apply Issues:**  
  Ensure your AWS credentials are valid and IAM permissions are sufficient.

- **Docker Push Issues:**  
  Ensure your DockerHub credentials are correct and have push access.

- **Ansible SSH Issues:**  
  Confirm Jenkins can SSH to your EC2s.

- **Port Conflicts:**  
  Ensure ports `8082`, `8085`, and `30007` are open in relevant firewalls/security groups.

- **Kubernetes:**  
  Make sure your cluster nodes can pull images from DockerHub and that `kubectl` is configured.

---

## Credits

- Terraform for AWS infrastructure automation
- Jenkins for CI/CD orchestration
- Docker for containerization
- Ansible for automated deployment
- Kubernetes for scalable orchestration
