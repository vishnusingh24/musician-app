pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "vishnusingh24/musician-app" // Change to your Docker Hub repo
        NODE_ENV = "production"
    }

    tools {
        nodejs "NodeJS"   // Ensure NodeJS is configured in Jenkins (Manage Jenkins > Global Tool Configuration)
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/vishnusingh24/musician-app.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                echo "Installing Node.js dependencies..."
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                echo "Running Jest tests..."
                sh 'npm test'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image..."
                sh "docker build -t ${DOCKER_IMAGE}:latest ."
            }
        }

        stage('Push Docker Image') {
            when {
                branch 'master'
            }
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
                        sh "docker push ${DOCKER_IMAGE}:latest"
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying the app (you can add kubectl, SSH, or docker-compose steps here)."
            }
        }
    }

    post {
        always {
            echo "Pipeline completed. Cleaning workspace..."
            cleanWs()
        }
        failure {
            echo "Pipeline failed!"
        }
        success {
            echo "Pipeline succeeded!"
        }
    }
}
