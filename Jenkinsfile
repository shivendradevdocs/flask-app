pipeline {
    agent {
        docker {
            image 'python:3.11' // Or any version you need
        }
    }
    stages {
        stage('Install Dependencies') {
            steps {
                sh 'pip install -r requirements.txt'
            }
        }
        stage('Run Tests') {
            steps {
                sh 'pytest'
            }
        }
    }
}
