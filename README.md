# ci-cd-pipeline
# Automated CI/CD Pipeline Using Jenkins & AWS

This project demonstrates the design and implementation of a complete Continuous Integration and Continuous Deployment (CI/CD) pipeline using **Jenkins** and **AWS EC2** to automate the build and deployment of a Java Spring Boot application.

---

## Project Overview

The objective of this project is to eliminate manual deployment processes by automating:

- Source code integration
- Build process
- Testing
- Deployment to AWS EC2

The pipeline ensures faster, consistent, and reliable deployments.

---

## Tech Stack

- Java
- Spring Boot
- Jenkins
- GitHub
- AWS EC2
- Maven
- Linux
- Git

---

## System Architecture

Developer → GitHub Repository → Jenkins → Build (Maven) → Deploy to AWS EC2 → Application Running

---

## CI/CD Workflow Explained

### 1️⃣ Code Push (Continuous Integration)

- Developer pushes code to GitHub.
- Jenkins monitors the repository.
- On detecting changes, Jenkins triggers the pipeline automatically.

---

### 2️⃣ Build Stage

Jenkins performs:

- Pull latest code from GitHub
- Run Maven build:

- Generate JAR file

If build fails → Pipeline stops  
If build succeeds → Move to deployment stage

---

### 3️⃣ Deployment Stage (Continuous Deployment)

- Jenkins connects to AWS EC2 instance via SSH.
- Transfers JAR file to EC2.
- Stops previous running instance (if any).
- Starts new application instance:

---

### 4️⃣ Application Live on AWS

- Application runs on EC2 instance.
- Accessible via:

- 
Infrastructure was provisioned, tested, and terminated after validation to optimize AWS costs.

---

## Jenkins Pipeline (Jenkinsfile)

```groovy
pipeline {
  agent any

  stages {

      stage('Checkout') {
          steps {
              git 'https://github.com/your-username/your-repo.git'
          }
      }

      stage('Build') {
          steps {
              sh 'mvn clean install'
          }
      }

      stage('Deploy') {
          steps {
              sh '''
              scp target/app.jar ec2-user@<EC2-IP>:/home/ec2-user/
              ssh ec2-user@<EC2-IP> "pkill -f app.jar || true"
              ssh ec2-user@<EC2-IP> "nohup java -jar app.jar &"
              '''
          }
      }
  }
}

