pipeline {
    agent any
    environment {
        // Define the base image name and tag using the Git commit SHA for unique identification
        IMAGE_NAME = 'beoboe/jenkins-docker-projet'
        IMAGE_TAG = "${IMAGE_NAME}:${env.GIT_COMMIT}"
    }
    stages {
        // Setup stage: Install necessary dependencies
        stage('Setup') {
            steps {
                sh "pip install -r requirements.txt --break-system-packages"
		sh "export PATH=$HOME/.local/bin:$PATH"
            }
        }
        // Test stage: Run tests using Pytest
        stage('Test') {
            steps {
                sh "pytest"
            }
        }
        // Docker Hub Login stage: Authenticate Jenkins with Docker Hub using secure credentials
        stage('Login to docker hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-creds', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh 'echo ${PASSWORD} | docker login -u ${USERNAME} --password-stdin'
                    echo 'Logged in successfully'
                }
            }
        }
        // Build Docker Image stage: Build the Docker image with the defined tag and verify its creation
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ${IMAGE_TAG} .'
                echo "Docker image built successfully"
                sh 'docker image ls'
            }
        }
        // Push Docker Image stage: Push the Docker image to Docker Hub
        stage('Push Docker Image') {
            steps {
                sh 'docker push ${IMAGE_TAG}'
                echo "Docker image pushed successfully"
            }
        }
    }
}
