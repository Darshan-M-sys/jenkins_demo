# Jenkins CI/CD Pipeline with Docker

## Project Overview

This project demonstrates a simple **CI/CD pipeline using Jenkins and Docker**.

The pipeline automatically:

1. Builds the application Docker image.
2. Tests the Docker image.
3. Deploys the application using a Docker container.

The project is designed to demonstrate how Jenkins can automate the build, test, and deployment process whenever code changes are pushed to GitHub.

---

## Technologies Used

* Jenkins
* Docker
* GitHub
* Git
* Nginx
* HTML
* Jenkins Pipeline
* Groovy

---

## Project Structure

```text
jenkins-cicd-demo/
│
├── app/
│   └── index.html
│
├── Dockerfile
├── Jenkinsfile
└── README.md
```

---

## Application

The application is a simple HTML webpage served using **Nginx**.

Example:

```html
<h1>Hello from Jenkins CI/CD!</h1>
<p>Application deployed automatically using Jenkins and Docker.</p>
```

---

## Dockerfile

The Dockerfile uses the lightweight Nginx Alpine image.

```dockerfile
FROM nginx:alpine

COPY app/index.html /usr/share/nginx/html/index.html

EXPOSE 80
```

### Docker Image

The Docker image is created using:

```bash
docker build -t jenkins-demo:latest .
```

### Run the Application

```bash
docker run -d --name jenkins-demo -p 8080:80 jenkins-demo:latest
```

The application can then be accessed at:

```text
http://localhost:8080
```

---

# Jenkins CI/CD Pipeline

The pipeline is defined in the `Jenkinsfile`.

The pipeline contains three main stages:

```text
Build
  ↓
Test
  ↓
Deploy
```

## Build Stage

The Build stage creates the Docker image.

```groovy
stage('Build') {
    steps {
        echo 'Building Docker image...'
        sh 'docker build -t jenkins-demo:latest .'
    }
}
```

## Test Stage

The Test stage checks the Nginx configuration inside the Docker image.

```groovy
stage('Test') {
    steps {
        echo 'Testing Docker image...'
        sh 'docker run --rm jenkins-demo:latest nginx -t'
    }
}
```

## Deploy Stage

The Deploy stage stops and removes the previous container and starts a new container.

```groovy
stage('Deploy') {
    steps {
        echo 'Deploying application...'

        sh '''
            docker stop jenkins-demo || true
            docker rm jenkins-demo || true

            docker run -d \
                --name jenkins-demo \
                -p 8080:80 \
                jenkins-demo:latest
        '''
    }
}
```

---

# Jenkins Configuration

The Jenkins job uses:

```text
Definition: Pipeline script from SCM
SCM: Git
Branch: */main
Script Path: Jenkinsfile
```

The Jenkins pipeline retrieves the project directly from the GitHub repository.

---

# Jenkins and Docker Permission

Jenkins runs using the Linux `jenkins` user.

To allow Jenkins to execute Docker commands, the Jenkins user was added to the Docker group:

```bash
sudo usermod -aG docker jenkins
```

Jenkins was then restarted:

```bash
sudo systemctl restart jenkins
```

Docker access was verified using:

```bash
sudo -u jenkins docker ps
```

---

# CI/CD Workflow

The complete workflow is:

```text
Developer
    │
    │ git push
    ↓
GitHub Repository
    │
    ↓
Jenkins
    │
    ├── Build
    │      └── Docker image
    │
    ├── Test
    │      └── Validate application
    │
    └── Deploy
           └── Docker container
                  │
                  ↓
             Application
```

---

# How to Run the Project

## 1. Clone the Repository

```bash
git clone <YOUR_GITHUB_REPOSITORY_URL>
```

```bash
cd jenkins-cicd-demo
```

## 2. Build Docker Image

```bash
docker build -t jenkins-demo:latest .
```

## 3. Run the Application

```bash
docker run -d --name jenkins-demo -p 8080:80 jenkins-demo:latest
```

## 4. Open the Application

Visit:

```text
http://localhost:8080
```

---

# Jenkins Pipeline Execution

After configuring the Jenkins Pipeline:

1. Open Jenkins.
2. Open the `jenkins-cicd-demo` job.
3. Click **Build Now**.
4. Open the build number.
5. Select **Console Output**.
6. Verify the Build, Test, and Deploy stages.

A successful pipeline should finish with:

```text
Finished: SUCCESS
```

---

# Automatic CI/CD

The pipeline can be configured to run automatically when changes are pushed to GitHub.

Example workflow:

```text
Developer changes code
        ↓
git add .
        ↓
git commit
        ↓
git push
        ↓
GitHub
        ↓
Jenkins triggered
        ↓
Build → Test → Deploy
```

This removes the need to manually perform the build and deployment process.

---

# Interview Questions

## 1. What is Jenkins, and how is it used in CI/CD?

Jenkins is an open-source automation server used to automate software development processes.

In CI/CD, Jenkins can automatically:

* Retrieve source code
* Build applications
* Run tests
* Build Docker images
* Deploy applications

This helps automate repetitive development and deployment tasks.

---

## 2. What is a Jenkinsfile?

A `Jenkinsfile` is a text file that defines a Jenkins Pipeline as code.

It describes:

* Pipeline configuration
* Stages
* Steps
* Build commands
* Test commands
* Deployment commands

Example:

```groovy
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building...'
            }
        }
    }
}
```

The Jenkinsfile can be stored inside the application's Git repository.

---

## 3. How do you create and configure Jenkins pipelines?

A basic process is:

1. Install Jenkins.
2. Create a Jenkins job.
3. Select **Pipeline**.
4. Connect the job to a Git repository.
5. Select **Pipeline script from SCM**.
6. Specify the branch.
7. Specify the Jenkinsfile path.
8. Save the configuration.
9. Run the pipeline.

The pipeline can also be configured with webhooks or polling to automatically trigger builds when code changes.

---

## 4. What are some common stages in a Jenkins pipeline?

Common stages include:

```text
Checkout
   ↓
Build
   ↓
Test
   ↓
Package
   ↓
Deploy
```

Depending on the project, additional stages can include:

* Code quality analysis
* Security scanning
* Docker image creation
* Integration testing
* Production deployment

---

## 5. What is the difference between Declarative and Scripted Jenkins Pipeline?

### Declarative Pipeline

Declarative Pipeline uses a structured syntax and is easier to read and maintain.

Example:

```groovy
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building application'
            }
        }
    }
}
```

### Scripted Pipeline

Scripted Pipeline uses Groovy programming more freely and provides greater flexibility for complex logic.

Example:

```groovy
node {
    stage('Build') {
        echo 'Building application'
    }
}
```

### Main Difference

| Declarative          | Scripted                        |
| -------------------- | ------------------------------- |
| Structured syntax    | Flexible Groovy code            |
| Easier for beginners | More programming-oriented       |
| Easier to maintain   | More flexible for complex logic |
| Uses `pipeline {}`   | Usually uses `node {}`          |

---

# Task Outcome

This project demonstrates a basic Jenkins CI/CD workflow using Docker.

The pipeline automates:

```text
Code
 ↓
Build
 ↓
Test
 ↓
Deploy
```

The project helped demonstrate how Jenkins can be integrated with GitHub and Docker to automate application delivery.

---

## Author

**Darshan**

GitHub: https://github.com/Darshan-M-sys
