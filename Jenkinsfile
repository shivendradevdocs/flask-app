pipeline {
    // agent { 
    //     label 'awsagent' 
    //   }
    agent any
    options{
        timeout(time: 3, unit: 'MINUTES')
    }

    stages {

        stage('Checkout') {
            steps {
                git url: 'https://github.com/kodekloudhub/jenkins-project.git', branch: 'main'
                sh "ls -ltr"
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
