pipeline {
    agent any

    environment {
        CLIENT_IMAGE = "client:${BUILD_NUMBER}"
        SERVER_IMAGE = "server:${BUILD_NUMBER}"
    }

    stages {
        stage('Initialize') {
            steps {
                echo '✅ Checking workspace contents and Git status...'
                sh 'pwd'
                sh 'ls -la'
                sh 'ls -la .git || echo ".git folder not found!"'
                sh 'git status || echo "Not a Git repository!"'
            }
        }

        stage('Docker Compose Up') {
            steps {
                script {
                    sh """
                        echo '🔧 Building and starting containers...'
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
