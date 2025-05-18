pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'manvisupriya/portfolio:latest'
        KUBE_CONFIG = 'C:\\Users\\YourUsername\\.kube\\config' // Update this path
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/1DS22CS222-SUPRIYA/portfolio.git', branch: 'main'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t %DOCKER_IMAGE% .'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    bat 'docker login -u %DOCKER_USER% -p %DOCKER_PASS%'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                bat 'docker push %DOCKER_IMAGE%'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                withEnv(["KUBECONFIG=${env.KUBE_CONFIG}"]) {
                    bat 'kubectl apply -f k8s-deployment.yaml'
                    bat 'kubectl apply -f k8s-service.yaml'
                }
            }
        }
    }
}
