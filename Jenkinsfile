pipeline {
  agent any

  environment {
    DEPLOY_SERVER = "ubuntu@<TARGET-EC2-IP>"
    SSH_KEY = credentials('deploy-key')  // You’ll add this in Jenkins credentials
  }

  stages {

    stage('Checkout Code') {
      steps {
        echo "Cloning GitHub repository..."
        git branch: 'main', url: 'https://github.com/rieckace/jenkinswebapp.git'
      }
    }

    stage('Build') {
      steps {
        echo "Installing dependencies..."
        sh 'npm install'
      }
    }

    stage('Test') {
      steps {
        echo "Running tests..."
        sh 'npm test'
      }
    }

    stage('Deploy') {
      steps {
        echo "Deploying application to EC2..."
        sshagent (credentials: ['deploy-key']) {
          sh '''
            scp -o StrictHostKeyChecking=no -r * ${DEPLOY_SERVER}:/var/www/html/
          '''
        }
      }
    }
  }

  post {
    success {
      echo "✅ Deployment successful!"
    }
    failure {
      echo "❌ Build failed. Check Jenkins logs."
    }
  }
}

