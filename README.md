# YouTube Clone — End-to-End DevOps CI/CD on AWS EKS

An end-to-end DevOps learning project focused on automating the build, security scanning, containerization, and Kubernetes deployment of a YouTube clone application using Jenkins and AWS.

## Project Overview

This project demonstrates a Jenkins-based CI/CD pipeline for a YouTube clone application. The pipeline integrates SonarQube for code quality analysis, Trivy for filesystem and container image scanning, Docker for containerization, Docker Hub for image publishing, and Kubernetes for application deployment.

The application is deployed on Amazon EKS, with Jenkins automating the deployment workflow and sending email notifications containing build information and available scan reports.

**Note:** The application source code is based on an existing project. My work focuses on the DevOps pipeline, deployment configuration, infrastructure integration, and troubleshooting performed as part of this learning project.

## Architecture

```text
GitHub Repository
       |
       v
Jenkins CI/CD Pipeline
       |
       v
Workspace Cleanup and Checkout
       |
       v
SonarQube Analysis
       |
       v
Quality Gate Check
       |
       v
Install npm Dependencies
       |
       v
Trivy Filesystem Scan
       |
       v
Docker Build and Push
       |
       v
Docker Hub Image
abin89/youtube-clone:latest
       |
       v
Trivy Image Scan
       |
       v
Kubernetes Deployment
       |
       v
Amazon EKS
       |
       v
Application Service
       |
       v
Email Build Notification
```

## Technology Stack

| Technology         | Purpose                                        |
| ------------------ | ---------------------------------------------- |
| GitHub             | Source code management                         |
| Jenkins            | CI/CD pipeline automation                      |
| Java JDK 17        | Configured Jenkins build tool                  |
| Node.js 16 and npm | JavaScript runtime and dependency installation |
| SonarQube          | Static code quality analysis                   |
| Trivy              | Filesystem and Docker image scanning           |
| Docker             | Application containerization                   |
| Docker Hub         | Container image registry                       |
| Kubernetes         | Application deployment and orchestration       |
| Amazon EKS         | Managed Kubernetes environment                 |
| kubectl            | Kubernetes resource management                 |

## Jenkins CI/CD Pipeline

The Jenkins pipeline automates the following stages:

### 1. Clean Workspace

Deletes existing workspace contents before the pipeline starts.

### 2. Checkout Source Code

Checks out the `main` branch from the GitHub repository.

### 3. SonarQube Analysis

Runs SonarScanner with the configured SonarQube server and project settings to analyze code quality.

### 4. Quality Gate

Waits for the SonarQube quality gate result. The current pipeline uses `abortPipeline: false`, meaning a failed quality gate does not automatically stop the pipeline at this stage.

### 5. Install Dependencies

Runs `npm install` to install the application's Node.js dependencies.

### 6. Trivy Filesystem Scan

Scans the project filesystem and saves the output to `trivyfs.txt`.

### 7. Docker Build and Push

Builds a Docker image and pushes it to Docker Hub using the configured Jenkins registry credentials.

Image reference:

`abin89/youtube-clone:latest`

### 8. Trivy Image Scan

Scans the Docker image and saves the output to `trivyimage.txt`.

### 9. Deploy to Kubernetes

Uses Jenkins Kubernetes credentials to connect to the configured cluster and applies the Kubernetes deployment and service manifests from the `Kubernetes/` directory.

### 10. Email Notification

Sends a build notification containing the job name, build number, status, build URL, and console log. The configured Trivy reports are attached when those files are available.

## Kubernetes Deployment

The deployment stage uses these manifest files:

* `Kubernetes/deployment.yml`
* `Kubernetes/service.yml`

Useful verification commands:

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

Use the appropriate namespace if the application is deployed elsewhere.

## Repository Structure

```text
a-youtube-clone-app/
├── README.md
├── Dockerfile
├── Jenkinsfile
├── Kubernetes/
│   ├── deployment.yml
│   └── service.yml
├── screenshots/
│   ├── application-homepage.png
│   ├── jenkins-pipeline.png
│   ├── kubernetes-pods.png
│   └── trivy-scan-results.png
└── .gitignore
```

This is a suggested structure based on the Jenkinsfile. Preserve the existing application source folders and include the screenshots only after uploading them. Keep sensitive files and credentials out of the repository.

## Security Considerations

* Jenkins credentials are used for Docker Hub and Kubernetes access.
* Trivy scan reports are generated for the filesystem and Docker image.
* SonarQube is integrated for code quality analysis.
* The current quality gate configuration does not automatically abort the pipeline when the gate fails.
* Review scan results and configure appropriate failure thresholds before treating the pipeline as a security enforcement mechanism.

## Challenges and Learnings

This project provided hands-on practice with:

* Automating build and deployment workflows using Jenkins.
* Integrating code quality analysis and vulnerability scanning.
* Building and publishing Docker images.
* Deploying applications to Kubernetes on AWS EKS.
* Managing CI/CD credentials through Jenkins.
* Troubleshooting Kubernetes resources and AWS LoadBalancer connectivity.

## Original Project Attribution

The application source code is based on the original project by [Ashfaque-9x](https://github.com/Ashfaque-9x).

* **Original repository:** https://github.com/Ashfaque-9x/a-youtube-clone-app
* **My repository:** https://github.com/abin7890/a-youtube-clone-app

Credit for the original application belongs to its original creator. Refer to the original repository's license and notices for the applicable reuse and attribution terms.

## Author

**Abin VA**

* GitHub: https://github.com/abin7890
* LinkedIn: https://www.linkedin.com/in/abin-va-37b502354
