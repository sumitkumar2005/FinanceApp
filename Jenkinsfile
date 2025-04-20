pipeline {
    agent any

    environment {
        CLIENT_IMAGE = 'client'
        SERVER_IMAGE = 'server'
    }

    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/sumitkumar2005/FinanceApp.git'
            }
        }

        stage('Build Client Docker Image') {
            steps {
                dir('client') {
                    script {
                        // Use double quotes for env variable expansion
                        sh "docker build -t ${CLIENT_IMAGE} ."
                    }
                }
            }
        }

        stage('Build Server Docker Image') {
            steps {
                dir('server') {
                    script {
                        sh "docker build -t ${SERVER_IMAGE} ."
                    }
                }
            }
        }

        stage('Docker Compose Up') {
            steps {
                script {
                    // Ensure we're in the project root
                    sh "docker-compose up -d --build"
                }
            }
        }
    }

    post {
        success {
            echo '✅ Deployment Successful!'
        }
        failure {
            echo '❌ Build Failed!'
        }
    }
}
