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

        stage('Build Docker Images') {
            parallel {
                stage('Build Client Image') {
                    steps {
                        dir('client') {
                            sh "docker build -t ${CLIENT_IMAGE} ."
                        }
                    }
                }
                stage('Build Server Image') {
                    steps {
                        dir('server') {
                            sh "docker build -t ${SERVER_IMAGE} ."
                        }
                    }
                }
            }
        }

        stage('Docker Compose Up') {
            steps {
                script {
                    // Pass image tags as env vars for dynamic usage in docker-compose
                    sh """
                    CLIENT_IMAGE=${CLIENT_IMAGE} \
                    SERVER_IMAGE=${SERVER_IMAGE} \
                    docker-compose -f docker-compose.prod.yml up -d --build
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
