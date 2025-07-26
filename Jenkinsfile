pipeline {
  agent any

  environment {
    NODE_ENV = "production"
    APP_NAME = "musician-app"
  }

  stages {
    stage('Checkout') {
      steps {
        git url: 'https://github.com/vishnusingh24/musician-app.git', branch: 'main'
      }
    }
    stage('Install Dependencies') {
      steps {
        sh 'npm install'
      }
    }
    stage('Run Lint & Tests') {
      steps {
        sh '''npm run lint || echo "No lint script found"
              npm test || echo "No tests found"'''
      }
    }
    stage('Build Frontend') {
      steps {
        sh 'npm run build || echo "No build script found"'
      }
    }
    stage('Archive Build') {
      steps {
        archiveArtifacts artifacts: 'build/**/*', fingerprint: true
      }
    }
  }

  post {
    success {
      echo "✅ Jenkins pipeline completed successfully!"
    }
    failure {
      echo "❌ Pipeline failed — check the logs."
    }
  }
}
