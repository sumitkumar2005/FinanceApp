pipeline {
    agent any

    // Prevent Jenkins from skipping the default Git checkout
    options {
        skipDefaultCheckout(false)
    }

    environment {
        CLIENT_IMAGE = "client:${BUILD_NUMBER}"
        SERVER_IMAGE = "server:${BUILD_NUMBER}"
    }

    stages {
        stage('Checkout') {
            steps {
                echo '📥 Cloning the Git repository...'
                // Explicit Git checkout (replace "main" with your actual branch if needed)
                git branch: 'main', url: 'https://github.com/sumitkumar2005/FinanceApp.git'
            }
        }

        stage('Initialize') {
            steps {
                echo '✅ Checking workspace and Git status...'
                sh 'pwd'
                sh 'ls -la'
                sh 'ls -la .git || echo ".git folder not found!"'
                sh 'git rev-parse --is-inside-work-tree || echo "Not in a Git repo!"'
                sh 'git config --get remote.origin.url || echo "Remote URL not set!"'
            }
        }

        stage('Docker Compose Up') {
            steps {
                script {
                    echo '🔧 Building and starting containers with Docker Compose...'
                    sh """
                        CLIENT_IMAGE=${CLIENT_IMAGE} SERVER_IMAGE=${SERVER_IMAGE} docker-compose up -d --build
                    """
                }
            }
        }
    }

    post {
        success {
            echo '🎉 Deployment Successful!'
        }
        failure {
            echo '❌ Build Failed!'
        }
        cleanup {
            echo '🧹 Cleaning up Docker...'
            sh 'docker system prune -f || true'
        }
    }
}
