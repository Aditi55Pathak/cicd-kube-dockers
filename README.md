# CI/CD for Docker Kubernetes using Jenkins

## Project Title
CI/CD for Docker Kubernetes using Jenkins

## Scenario
The project involves setting up a continuous integration and continuous delivery (CI/CD) pipeline for an application with a microservices architecture. The application’s components are containerized, and developers are constantly making changes to improve development. Consequently, the operations (ops) team frequently needs to adapt to these changes, requiring a continuous build and test process along with regular container builds. Additionally, the ops team must manage regular deployment requests using a Kubernetes cluster to maintain the system.

## Problems
Due to continuous code changes, the ops team receives frequent deployment requests, making it a time-consuming process. The development (dev) team is dependent on the ops team for all deployments, creating a bottleneck and potentially slowing down the overall development and deployment cycle.

## Solution
To address these challenges, an automated build and release process is required. Implementing a continuous delivery pipeline for Docker containers will streamline the process, allowing for continuous integration, delivery, and deployment. This will reduce the dependency of the dev team on the ops team and speed up the overall workflow.

## Prerequisites
1. **Jenkins** - Installed and configured.
2. **Docker** - Installed and running on the Jenkins server.
3. **Kubernetes Cluster** - Access to a running Kubernetes cluster.
4. **Git** - Source code repository.
5. **Kubectl** - Kubernetes command-line tool configured to interact with your cluster.
6. **Helm** - Optional, for managing Kubernetes applications.

## Steps to Implement CI/CD Pipeline

### 1. Setup Jenkins
- Install necessary plugins:
  - Docker
  - Kubernetes
  - Git
  - Pipeline
  - Blue Ocean (optional, for a better visualization of the pipeline)
- Create a new Jenkins Pipeline job.

### 2. Create Jenkins Pipeline Script (Jenkinsfile)
Create a `Jenkinsfile` in your project repository:

```groovy
pipeline {
    agent any

    environment {
        registry = "your-docker-registry/your-app"
        registryCredential = 'docker-hub-credentials'
        dockerImage = ''
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://your-git-repository.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('', registryCredential) {
                        dockerImage.push()
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    kubectl.apply("--filename k8s/deployment.yaml")
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
```

### 3. Configure Kubernetes Deployment
Create a `deployment.yaml` file in the `k8s` directory of your project repository:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: your-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: your-app
  template:
    metadata:
      labels:
        app: your-app
    spec:
      containers:
      - name: your-app
        image: your-docker-registry/your-app:latest
        ports:
        - containerPort: 8080
```

### 4. Jenkins Credentials
- Add Docker Hub credentials in Jenkins:
  - Go to Jenkins Dashboard > Manage Jenkins > Manage Credentials > (select a domain) > Add Credentials.
  - Use these credentials in your `Jenkinsfile` as `registryCredential`.

### 5. Triggering the Pipeline
- Push changes to your Git repository to trigger the Jenkins pipeline.
- Jenkins will automatically build, push the Docker image, and deploy it to the Kubernetes cluster.

## Conclusion
By setting up a CI/CD pipeline using Jenkins for Docker and Kubernetes, you can automate the build, test, and deployment processes. This reduces the dependency on the ops team, speeds up development cycles, and ensures a consistent deployment process.

## Prerequisites
- JDK 1.8 or later
- Maven 3 or later
- MySQL 5.6 or later

## Technologies 
- Spring MVC
- Spring Security
- Spring Data JPA
- Maven
- JSP
- MySQL
## Database
Here,we used Mysql DB 
MSQL DB Installation Steps for Linux ubuntu 14.04:
- $ sudo apt-get update
- $ sudo apt-get install mysql-server

Then look for the file :
- /src/main/resources/accountsdb
- accountsdb.sql file is a mysql dump file.we have to import this dump to mysql db server
- > mysql -u <user_name> -p accounts < accountsdb.sql


