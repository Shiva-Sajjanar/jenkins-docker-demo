# Jenkins + Docker Automated CI/CD Pipeline

## 📌 Project Overview

This project demonstrates an automated Continuous Integration and Continuous Deployment (CI/CD) pipeline using Jenkins, GitHub, Docker, and Nginx.

Whenever new code is pushed to the GitHub repository, Jenkins checks for changes using Poll SCM and automatically triggers the pipeline. Jenkins builds a new Docker image, removes the old application container, and deploys a new container.

The deployed website can be accessed through a web browser.

## 🎯 Project Objectives

* Automate application deployment using Jenkins.
* Integrate Jenkins with GitHub.
* Build Docker images automatically.
* Deploy a website inside a Docker container.
* Verify the deployment using an HTTP request.
* Understand the CI/CD workflow used in DevOps.

## 🛠️ Technologies Used

| Technology   | Purpose                       |
| ------------ | ----------------------------- |
| Ubuntu (WSL) | Linux development environment |
| Git          | Version control               |
| GitHub       | Source code repository        |
| Jenkins      | CI/CD automation              |
| Docker       | Containerization              |
| Nginx        | Web server                    |
| HTML         | Website content               |

## 🏗️ Architecture

```text
Developer
    |
    | Update Code
    v
Git Repository
    |
    | Git Push
    v
GitHub Repository
    |
    | Poll SCM Detects Changes
    v
Jenkins Pipeline
    |
    | Stage 1: Build Docker Image
    v
Docker Image
    |
    | Stage 2: Stop and Remove Old Container
    v
Docker Container
    |
    | Stage 3: Deploy New Container
    v
Nginx Web Server
    |
    | Stage 4: Verify Deployment
    v
Website: http://localhost:8085
```

## 📂 Project Structure

```text
jenkins-docker-demo/
├── Dockerfile
├── Jenkinsfile
├── index.html
└── README.md
```

## ⚙️ Prerequisites

Install and configure the following tools:

* Ubuntu or another Linux environment
* Git
* Docker Engine
* Jenkins
* Java (required by Jenkins)

Ensure Jenkins has permission to execute Docker commands.

## 🐳 Dockerfile

The Dockerfile uses the lightweight Nginx Alpine image and copies the website HTML file into the Nginx web root.

```dockerfile
FROM nginx:alpine

COPY index.html /usr/share/nginx/html/index.html

EXPOSE 80
```

## 🔄 Jenkins Pipeline

The `Jenkinsfile` defines the automated deployment process.

```groovy
pipeline {
    agent any

    stages {
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t jenkins-docker-demo:v1 .'
            }
        }

        stage('Stop Old Container') {
            steps {
                sh 'docker rm -f jenkins-docker-website || true'
            }
        }

        stage('Deploy New Container') {
            steps {
                sh 'docker run -d --name jenkins-docker-website -p 8085:80 jenkins-docker-demo:v1'
            }
        }

        stage('Verify Website') {
            steps {
                sh 'sleep 3'
                sh 'curl --fail http://localhost:8085'
            }
        }
    }
}
```

### Pipeline Stages

1. **Build Docker Image:** Creates a Docker image from the Dockerfile.
2. **Stop Old Container:** Removes the existing application container if it exists.
3. **Deploy New Container:** Starts a new container using the newly built image.
4. **Verify Website:** Sends an HTTP request to verify that the application responds successfully.

## 🚀 Setup and Execution

### Step 1: Clone the Repository

```bash
git clone https://github.com/Shiva-Sajjanar/jenkins-docker-demo.git
```

### Step 2: Enter the Project Directory

```bash
cd jenkins-docker-demo
```

### Step 3: Build the Docker Image Manually

```bash
docker build -t jenkins-docker-demo:v1 .
```

### Step 4: Run the Container Manually

```bash
docker run -d --name jenkins-docker-website -p 8085:80 jenkins-docker-demo:v1
```

If the container name already exists, remove the old container before running this command.

### Step 5: Verify the Website

Open the following address in your browser:

**http://localhost:8085**

You can also test the application using:

```bash
curl http://localhost:8085
```

> The manual setup commands are for initial testing. In the configured CI/CD workflow, Jenkins performs the Docker build and deployment automatically.

## 🔧 Jenkins Configuration

Configure the Jenkins pipeline with these settings:

* **Job Name:** `docker-ci-cd-demo`
* **Pipeline Definition:** Pipeline script from SCM
* **SCM:** Git
* **Repository URL:** `https://github.com/Shiva-Sajjanar/jenkins-docker-demo.git`
* **Branch:** `*/main`
* **Script Path:** `Jenkinsfile`
* **Build Trigger:** Poll SCM
* **Polling Schedule:** `H/2 * * * *`

The polling schedule checks the repository approximately every two minutes. The actual time at which a build starts may vary.

## 🧪 Testing Automatic Deployment

1. Open the `index.html` file.
2. Update the website content.
3. Save the file.
4. Commit and push the changes to GitHub.

```bash
git add index.html
git commit -m "Update website content"
git push origin main
```

5. Wait for Jenkins to detect the change.
6. Open the Jenkins job and check the build status.
7. Verify the updated website at `http://localhost:8085`.

### Example HTML

```html
<h1>Hello from Jenkins + Docker CI/CD!</h1>
<p>My first automated deployment.</p>
<h2>Automatic Deployment is Working!</h2>
```

## ✅ Project Results

* Successfully integrated Jenkins with GitHub.
* Configured automatic builds using Poll SCM.
* Built Docker images through a Jenkins pipeline.
* Automated the replacement of the running application container.
* Deployed the website using an Nginx Docker container.
* Verified the updated application through a browser.
* Successfully tested the automatic deployment workflow.

## 📚 Key Learnings

* Git and GitHub integration
* Jenkins pipeline configuration
* Jenkinsfile and declarative pipelines
* Poll SCM and automated build triggers
* Docker image creation
* Docker container lifecycle management
* Automated application deployment
* HTTP-based application verification
* CI/CD troubleshooting

## 🔮 Future Improvements

* Configure GitHub Webhooks for event-driven builds.
* Add automated testing before deployment.
* Use versioned Docker image tags instead of a fixed tag.
* Store credentials securely using Jenkins Credentials.
* Implement rollback when a deployment fails.
* Add build notifications.
* Deploy the application to an AWS environment.
* Introduce security scanning and container image scanning.

## 👨‍💻 Author

**Shiva Sajjanar**

GitHub: [Shiva-Sajjanar](https://github.com/Shiva-Sajjanar)

Project Repository: [jenkins-docker-demo](https://github.com/Shiva-Sajjanar/jenkins-docker-demo)

