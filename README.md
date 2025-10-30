# Blue-Green Deployment with Jenkins CI/CD Pipeline

## Overview

This repository demonstrates a complete **CI/CD pipeline** implementation using **Jenkins** for a blue-green deployment strategy on Kubernetes. The project highlights an automated workflow that builds, tests, and deploys application updates with zero downtime, ensuring seamless transitions between application versions.

The pipeline is designed to reflect modern DevOps practices, integrating source control, containerization and Kubernetes deployment automation.

---

## Key Features

* **Jenkins Declarative Pipeline** defined via `Jenkinsfile`
* **GitHub Integration** with automated build triggers
* **Dockerized Application Build** using Jenkins build agents
* **Blue-Green Deployment** strategy for zero-downtime rollouts
* **Kubernetes Manifests** for scalable and resilient deployment management
* **Environment-Specific Configuration** for flexible deployment handling
* **Version Control Workflow** integrated with Jenkins automation

---

## Architecture

1. **Source Control:**
   The application source code and Jenkinsfile are hosted on GitHub. Jenkins is configured to poll the repository or trigger builds via webhook on code changes.

2. **Build Process:**
   Jenkins pulls the latest code, builds the Docker image, and tags it with a unique version (e.g., build number or Git commit hash).

3. **Image Storage:**
   The built image is pushed to a container registry (Docker Hub).

4. **Deployment Strategy:**
   The pipeline applies a **blue-green deployment** model:

   * “Blue” represents the current live environment.
   * “Green” is the new version under testing.
     Once validated, traffic is switched to the green environment.

5. **Kubernetes Deployment:**
   The manifests in the `k8s/` directory define the deployment, service, and ingress configurations for the application.

---

## Repository Structure

```
blue-green-cicd-demo/
│
├── app.py                       # Main application script
├── requirements.txt             # Python dependencies
├── Dockerfile                   # Docker build configuration
├── Jenkinsfile                  # Declarative Jenkins pipeline
├── k8s/                         # Kubernetes manifests for blue-green deployment
│   ├── blue-deployment.yaml
│   ├── green-deployment.yaml
│   ├── service.yaml
│   └── switch-to-green.yaml
└── README.md                    # Project documentation

```
---

## Jenkins Pipeline Flow

1. **Checkout Code:**
   Jenkins pulls the latest commit from the specified branch (default: `main`).

2. **Build and Tag Docker Image:**
   The Docker image is built and tagged using the Jenkins build number.

3. **Push to Registry:**
   The image is pushed to the configured container registry using stored credentials.

4. **Deploy to Kubernetes:**
   Jenkins applies Kubernetes manifests and updates the deployment using blue-green strategy.

5. **Post-Deployment Verification:**
   Health checks and validation ensure successful deployment before switching traffic.

---

## Tech Stack

* **Jenkins** – CI/CD automation server
* **Docker** – Containerization platform
* **Kubernetes (Minikube)** – Deployment and service orchestration
* **GitHub** – Source code management
* **Shell/Bash** – Build and deploy scripting
* **YAML** – Configuration management for Kubernetes and pipeline definitions

---

## Prerequisites

* Jenkins server (configured with Docker and Kubernetes CLI access)
* Minikube or Kubernetes cluster
* GitHub repository linked to Jenkins
* Docker registry credentials configured in Jenkins

---

## Setup Instructions

1. Clone the repository:

  cbash
   git clone https://github.com/Extraordinarytechy/blue-green-cicd-demo.git
   cd blue-green-cicd-demo
   ```

2. Configure Jenkins:

   * Set up a **new pipeline job**.
   * Choose **“Pipeline script from SCM”**.
   * Use this repository URL.
   * Specify the branch: `main`.

3. Add credentials (if required):

   * Docker registry credentials.
   * GitHub token (for private repo access).

4. Build the pipeline:

   * Trigger a manual build or configure a GitHub webhook for automatic builds.

5. Verify deployment:

   * Access the deployed application via the Kubernetes ingress or service URL.

---

## Blue-Green Deployment Example

During a new deployment:

* The **green** deployment is created alongside the active **blue** deployment.
* Once the new version is validated, traffic is switched from blue to green.
* The blue deployment remains on standby for rollback purposes.

This ensures **zero downtime** and enables **safe rollback** in case of issues.

---

## Future Enhancements

* Integrate automated testing and code quality analysis (SonarQube)
* Add Helm for advanced Kubernetes packaging
* Implement rollback automation using Jenkins post-deployment scripts
* Integrate monitoring with Prometheus and Grafana

---

## Author

**Ajay kumar [Extraordinarytechy]**
Cloud & DevOps engineer

