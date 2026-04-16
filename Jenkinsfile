pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "tejasise/app-image"
    }

    stages {

        stage('Clone Repository') {
            steps {
                git 'https://github.com/TejasMK2005/dock.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}:latest")
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
        // 'docker-hub-creds' would be the ID of a "Username with password" credential
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', 
                                                  passwordVariable: 'DOCKER_PASS', 
                                                  usernameVariable: 'DOCKER_USER')]) {
                    bat "docker login -u %DOCKER_USER% -p %DOCKER_PASS%"
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('', 'dockerhub-creds') {
                        docker.image("${DOCKER_IMAGE}:latest").push()
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Image successfully built and pushed to Docker Hub'
        }
        failure {
            echo 'Pipeline failed'
        }
    }
}
