pipeline {
    agent any

    environment {
        PATH = "/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin"
    }

    stages {

        stage('Checkout Code') {
            steps {
                echo 'Code checked out from GitHub'
            }
        }

        stage('Check Docker') {
            steps {
                sh 'docker --version'
            }
        }

        stage('Build Backend Image') {
            steps {
                sh 'docker build -t vansh2083/restaurant-reservation-backend:latest backend'
            }
        }

        stage('Build Frontend Image') {
            steps {
                sh 'docker build -t vansh2083/restaurant-reservation-frontend:latest frontend'
            }
        }

        stage('Push Images to Docker Hub') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'docker-hub-credentials',
                        usernameVariable: 'DOCKER_USERNAME',
                        passwordVariable: 'DOCKER_PASSWORD'
                    )
                ]) {
                    sh '''
                        echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin
                        docker push vansh2083/restaurant-reservation-backend:latest
                        docker push vansh2083/restaurant-reservation-frontend:latest
                        docker logout
                    '''
                }
            }
        }

        stage('Deploy Locally') {
            steps {
                sh '''
                    docker compose pull backend frontend
                    docker compose up -d backend frontend
                '''
            }
        }
    }
}
