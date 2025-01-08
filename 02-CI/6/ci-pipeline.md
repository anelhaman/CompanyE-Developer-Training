## Initialize Java Spring Boot Project and React App with Jenkinsfile

1. **Jenkinsfile for Java Spring Boot Project**
This Jenkinsfile will build the Java Spring Boot project using Maven, create a Docker image, and push it to Docker Hub.
Add the following to your `Jenkinsfile` in the root directory:

```groovy
pipeline {
    agent any

    environment {
        DOCKER_IMAGE_NAME = "your-dockerhub-username/my-java-app"
        DOCKER_TAG = "latest"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/your-username/my-java-app.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    docker.build(DOCKER_IMAGE_NAME, "-f Dockerfile .")
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
                        docker.image(DOCKER_IMAGE_NAME).push(DOCKER_TAG)
                        docker.image(DOCKER_IMAGE_NAME).push(BUILD_NUMBER)
                    }
                }
            }
        }
    }
}
```
---

2. **Jenkinsfile for React App**
This Jenkinsfile will build the React app, create a Docker image, and push it to Docker Hub.
Add the following to your `Jenkinsfile` in the root directory of your React project:

```groovy
pipeline {
    agent any

    environment {
        DOCKER_IMAGE_NAME = "your-dockerhub-username/my-react-app"
        DOCKER_TAG = "latest"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/your-username/my-react-app.git'
            }
        }

        stage('Build') {
            steps {
                sh 'npm install'
                sh 'npm run build'
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    docker.build(DOCKER_IMAGE_NAME, "-f Dockerfile .")
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
                        docker.image(DOCKER_IMAGE_NAME).push(DOCKER_TAG)
                        docker.image(DOCKER_IMAGE_NAME).push(BUILD_NUMBER)
                    }
                }
            }
        }
    }
}
```
