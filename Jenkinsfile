pipeline {
  agent any

  stages {
    stage('Checkout Code') {
      steps {
        echo "📦 Cloning GitHub repository..."
        git branch: 'main', url: 'https://github.com/rieckace/jenkinswebapp.git'
      }
    }

    stage('Build') {
      steps {
        echo "⚙️ Build stage: (No build needed for static HTML site)"
        sh 'echo "No build steps required."'
      }
    }

    stage('Test') {
      steps {
        echo "🧪 Running tests (skipping for static HTML)..."
        sh 'echo "No tests defined."'
      }
    }

    stage('Deploy to Nginx') {
  steps {
    echo "🚀 Deploying files to Nginx web directory..."
    sh '''
      sudo rm -rf /var/www/html/*
      sudo cp -r * /var/www/html/
      sudo chown -R www-data:www-data /var/www/html
      sudo chmod -R 755 /var/www/html

      # Add a timestamp footer to the HTML page
      DEPLOY_TIME=$(date +"%Y-%m-%d %H:%M:%S UTC")
      echo "<footer style='text-align:center; margin-top:20px; font-size:14px; color:#888;'>Last deployed on: $DEPLOY_TIME</footer>" | sudo tee -a /var/www/html/index.html

      sudo systemctl restart nginx
      echo "✅ Deployment complete at $DEPLOY_TIME" | sudo tee /var/www/html/deploy.log
    '''
    }
  }
} // ✅ closes stages

  post {
    success {
      echo "✅ Deployment successful! Visit http://<YOUR-EC2-PUBLIC-IP>"
    }
    failure {
      echo "❌ Deployment failed. Check Jenkins logs for details."
    }
  }
}
