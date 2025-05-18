pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = 'dockerhub-creds'  // Jenkins DockerHub credentials ID
        IMAGE_NAME = 'manvisupriya/portfolio'
        IMAGE_TAG = 'latest'
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/1DS22CS222-SUPRIYA/portfolio.git', credentialsId: 'github-token', branch: 'main'
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(credentialsId: env.DOCKERHUB_CREDENTIALS, usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    bat """
                    echo %PASSWORD% | docker login -u %USERNAME% --password-stdin
                    """
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${env.IMAGE_NAME}:${env.IMAGE_TAG}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.image("${env.IMAGE_NAME}:${env.IMAGE_TAG}").push()
                }
            }
        }

        stage('Cleanup') {
            steps {
                bat "docker rmi ${env.IMAGE_NAME}:${env.IMAGE_TAG}"
            }
        }
    }

    post {
        always {
            bat 'docker logout'
        }
    }
}
