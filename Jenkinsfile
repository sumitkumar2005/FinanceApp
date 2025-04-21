pipeline {
    agent any

    environment {
        CLIENT_IMAGE = "client:${BUILD_NUMBER}"
        SERVER_IMAGE = "server:${BUILD_NUMBER}"
    }

    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/sumitkumar2005/FinanceApp.git'
            }
        }

        stage('Docker Compose Up') {
            steps {
                script {
                    sh """
                    export CLIENT_IMAGE=${CLIENT_IMAGE}
                    export SERVER_IMAGE=${SERVER_IMAGE}
                    docker-compose up -d --build
                    """
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment Successful!'
        }
        failure {
            echo 'Build Failed!'
        }
        cleanup {
            echo 'Cleaning up Docker'
            sh 'docker system prune -f'
        }
    }
}
