# YouTube Clone — End-to-End DevSecOps Project on AWS EKS

An end-to-end DevOps project demonstrating CI/CD automation, infrastructure as code, containerization, security scanning, Kubernetes deployment, and monitoring using AWS and open-source tools.
<p align="center">
  <img src="DevSecOps%20CI_CD%20Pipeline%20on%20AWS%20EKS.png"
       alt="DevSecOps CI/CD Pipeline Architecture"
       width="550">
</p>

## Project Overview

This project focuses on deploying a YouTube clone application on Amazon EKS using a Jenkins-based CI/CD pipeline.

The pipeline integrates GitHub for source control, SonarQube for code quality analysis, Trivy for filesystem and Docker image scanning, Docker for containerization, and Docker Hub for image publishing. Kubernetes is used to deploy the application, while Prometheus and Grafana provide monitoring capabilities.

Terraform is used for infrastructure as code, and Jenkins email notifications provide build status and scan reports.

**Note:** The original application source code is based on another developer's repository. This project showcases my DevOps implementation and deployment work.

## Architecture

The project follows this workflow:

```text
Developer
    |
    v
Git / GitHub Repository.
    |
    v
Jenkins CI/CD Pipeline
    |
    +----> SonarQube Code Analysis
    |
    +----> npm Dependency Installation
    |
    +----> Trivy Filesystem Scan
    |
    +----> Docker Build
    |          |
    |          v
    |      Docker Hub
    |
    +----> Trivy Image Scan
    |
    +----> Kubernetes Deployment
               |
               v
         Amazon EKS
               |
               v
       Application Service

Terraform ---> AWS Infrastructure

Prometheus ---> Metrics Collection
Grafana ------> Monitoring Dashboards

Jenkins ---> Email Notifications
```

The architecture diagram represents the overall project. The Jenkinsfile defines the automated pipeline stages, while Terraform and monitoring components are configured separately.

## Technologies Used

| Technology                  | Purpose                                          |
| --------------------------- | ------------------------------------------------ |
| Git and GitHub              | Source code management                           |
| Jenkins                     | CI/CD pipeline automation                        |
| SonarQube                   | Static code quality analysis                     |
| npm                         | JavaScript dependency installation               |
| Trivy                       | Filesystem and container image security scanning |
| Docker                      | Application containerization                     |
| Docker Hub                  | Container image registry                         |
| Terraform                   | Infrastructure as code                           |
| Amazon EKS                  | Managed Kubernetes service                       |
| Kubernetes                  | Application deployment and orchestration         |
| Prometheus                  | Metrics collection and monitoring                |
| Grafana                     | Monitoring dashboards and visualization          |
| Gmail / Email notifications | Jenkins build status and report delivery         |

## CI/CD Pipeline Stages

### 1. Workspace Cleanup

Jenkins cleans the workspace before starting a new pipeline execution.

### 2. Source Code Checkout

The pipeline checks out the `main` branch from the GitHub repository.

### 3. SonarQube Analysis

SonarScanner analyzes the application source code using the configured SonarQube server and project settings.

### 4. Quality Gate

Jenkins waits for the SonarQube quality gate result. The current pipeline is configured with `abortPipeline: false`, so a failed quality gate does not automatically stop subsequent stages.

### 5. Install Dependencies

The pipeline runs `npm install` to install application dependencies.

### 6. Trivy Filesystem Scan

Trivy scans the project filesystem. The output is saved to `trivyfs.txt`.

### 7. Docker Build and Push

Jenkins builds a Docker image and pushes it to Docker Hub using configured Jenkins credentials.

Docker image:

`abin89/youtube-clone:latest`

### 8. Trivy Image Scan

Trivy scans the built Docker image and saves the results to `trivyimage.txt`.

### 9. Deploy to Kubernetes

Jenkins connects to the configured Kubernetes cluster and applies the deployment and service manifests located in the `Kubernetes/` directory.

* `deployment.yml`
* `service.yml`

The application is deployed on Amazon EKS.

### 10. Email Notifications

Jenkins sends an email containing the job name, build number, build status, and build URL. The console log and available Trivy reports are attached according to the pipeline configuration.

## Infrastructure as Code with Terraform

Terraform is used in the project to manage AWS infrastructure through configuration files.

Infrastructure as code helps make resource configuration repeatable and easier to maintain.

Document the specific resources you provisioned, such as the VPC, subnets, security groups, EKS cluster, or worker nodes, based on your actual Terraform configuration.

## Kubernetes Deployment

The application runs on Kubernetes within Amazon EKS.

Useful commands for inspecting the deployment:

```bash
kubectl get nodes
kubectl get pods -n default
kubectl get deployments -n default
kubectl get services -n default
```

Inspect the application service:

```bash
kubectl describe svc youtube-clone-app -n default
```

Use the appropriate namespace if your application is deployed elsewhere.

## Monitoring with Prometheus and Grafana

Prometheus and Grafana are used for monitoring in this project.

* **Prometheus:** Collects metrics from configured monitoring targets.
* **Grafana:** Visualizes metrics through dashboards.
  
## 📸 Project Screenshots

<p>These screenshots showcase my end-to-end DevSecOps CI/CD pipeline, including code analysis, containerization, AWS infrastructure, Kubernetes deployment, and application deployment.</p>

<h3>1. Jenkins CI/CD Pipeline</h3>
<img src="./screenshots/Jenkins%20CICD%20Pipeline.png" alt="Jenkins CI/CD Pipeline" width="500">

<h3>2. SonarQube Code Analysis</h3>
<img src="./screenshots/SonarQube%20Code%20Analysis.png" alt="SonarQube Code Analysis" width="500">

<h3>3. Docker Image</h3>
<img src="./screenshots/Docker%20Image.png" alt="Docker Image" width="500">

<h3>4. Terraform and AWS Infrastructure</h3>
<img src="./screenshots/Terraform%20and%20AWS%20Infrastructure.png" alt="Terraform and AWS Infrastructure" width="500">

<h3>5. Kubernetes Deployments</h3>
<img src="./screenshots/Kubernetes%20Deployments.png" alt="Kubernetes Deployments" width="500">

<h3>6. Deployed Application</h3>
<img src="./screenshots/Deployed%20Application.png" alt="Deployed Application" width="500">


## Key Learnings

* Automating application delivery using Jenkins CI/CD pipelines.
* Integrating static code analysis and security scanning into CI/CD.
* Building and publishing Docker images to Docker Hub.
* Deploying containerized applications on Amazon EKS.
* Managing infrastructure using Terraform.
* Monitoring configured application and infrastructure metrics using Prometheus and Grafana.
* Troubleshooting Kubernetes resources and AWS LoadBalancer connectivity.

## Original Project Attribution

The application source code is based on the original project by [Ashfaque-9x](https://github.com/Ashfaque-9x).

* **Original repository:** https://github.com/Ashfaque-9x/a-youtube-clone-app
* **My repository:** https://github.com/abin7890/a-youtube-clone-app

Credit for the original application belongs to its original creator. Refer to the original repository's license and notices for applicable reuse and attribution requirements.

## Author

**Abin VA**

* GitHub: https://github.com/abin7890
* LinkedIn: https://www.linkedin.com/in/abin-va-37b502354
