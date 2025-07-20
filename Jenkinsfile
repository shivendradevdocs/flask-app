pipeline {
    agent {
        docker { 
          image 'python:3.10-slim'     // official Python image with pip included
          args  '--user root'          // run build steps as root so pip can install globally
        }
      }

    stages {

        stage('Checkout') {
            steps {
                git url: 'https://github.com/kodekloudhub/jenkins-project.git', branch: 'main'
                sh "ls -ltr"
            }
        }
        stage('Install deps') {
          steps {
        // use python3 -m pip so we don’t rely on a standalone 'pip' binary
                sh 'python3 -m pip install --upgrade pip'
                sh 'python3 -m pip install -r requirements.txt'
      }
    }
        stage('Setup') {
            steps {
                sh "pip install -r requirements.txt"
            }
        }
        stage('Test') {
            steps {
                sh "pytest"
                sh "whoami"
            }
        }


    }
}
