// pipeline {
//     agent any

//     environment {
//         IMAGE_NAME = "simple-webapp"
//         CONTAINER_NAME = "webapp-container"
//     }

//     stages {
//         stage('Checkout Code') {
//             steps {
//                 git branch: 'main', url: 'https://github.com/rieckace/jenkinswebapp.git'
//             }
//         }

//         stage('Build Docker Image') {
//             steps {
//                 script {
//                     sh "docker build -t ${IMAGE_NAME}:latest ."
//                 }
//             }
//         }

//         stage('Deploy Container') {
//             steps {
//                 script {
//                     // Stop and remove old container if running
//                     sh """
//                     docker rm -f ${CONTAINER_NAME} || true
//                     docker run -d --name ${CONTAINER_NAME} -p 80:80 ${IMAGE_NAME}:latest
//                     """
//                 }
//             }
//         }
//     }
// }


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
        git branch: 'main', url: 'https://github.com/<your-username>/<your-repo>.git'
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

