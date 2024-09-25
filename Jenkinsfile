pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'hisanusman/flasktest-app'
        DOCKERHUB_CREDENTIALS = 'dockerhub-creds-id'
    }

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}:${BUILD_NUMBER}")
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', DOCKERHUB_CREDENTIALS) {}
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.image("${DOCKER_IMAGE}:${BUILD_NUMBER}").push()
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
