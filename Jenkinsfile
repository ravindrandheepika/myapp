pipeline {
    agent any

    environment {
        DOCKERHUB_USER = "dheepika96"
        DOCKER_IMAGE = "myapp"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git 'https://github.com/ravindrandheepika/myapp.git'
            }
        }

        stage('Build Java Project') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ${DOCKERHUB_USER}/${DOCKER_IMAGE}:latest .'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds',
                                                 usernameVariable: 'USERNAME',
                                                 passwordVariable: 'PASSWORD')]) {
                    sh 'echo $PASSWORD | docker login -u $USERNAME --password-stdin'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                sh 'docker push ${DOCKERHUB_USER}/${DOCKER_IMAGE}:latest'
            }
        }
    }
}
