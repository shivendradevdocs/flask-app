pipeline {
  agent any

  options {
    timeout(time: 3, unit: 'MINUTES')
  }

  environment {
    SERVER_IP = credentials('prod-server-ip')
  }

  stages {

    stage('Checkout') {
      steps {
        git url: 'https://github.com/kodekloudhub/jenkins-project.git', branch: 'main'
        sh 'ls -ltr'
      }
    }

    stage('Setup') {
      steps {
        sh 'pip install -r requirements.txt'
      }
    }

    stage('Test') {
      steps {
        sh 'pytest'
        sh 'whoami'
      }
    }

    stage('Package Code') {
      steps {
        sh "zip -r myapp.zip ./* -x '*.git*'"
        sh 'ls -lart'
      }
    }

    stage('Deploy to Prod') {
      steps {
        withCredentials([
          sshUserPrivateKey(
            credentialsId: 'ssh-key',
            keyFileVariable: 'MY_SSH_KEY',
            usernameVariable: 'username'
          )
        ]) {
          sh '''
            # Copy the app zip to the remote server
            scp -i $MY_SSH_KEY -o StrictHostKeyChecking=no myapp.zip ${username}@${SERVER_IP}:/home/ec2-user/

            # SSH into the remote server and deploy the app
            ssh -i $MY_SSH_KEY -o StrictHostKeyChecking=no ${username}@${SERVER_IP} <<EOF
cd /home/ec2-user/app/
unzip -o /home/ec2-user/myapp.zip -d /home/ec2-user/app/
source venv/bin/activate
pip install -r requirements.txt
sudo systemctl restart flaskapp.service
EOF
          '''
        }
      }
    }

  }

  post {
    failure {
      echo '❌ Deployment failed!'
    }
    success {
      echo '✅ Deployment succeeded!'
    }
  }
}
