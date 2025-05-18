pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds') // Jenkins credentials ID
        IMAGE_NAME = "manvisupriya/portfolio"
        KUBE_CONFIG = credentials('kubeconfig') // optional, if using kubeconfig in Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/1DS22CS222-SUPRIYA/portfolio.git',branch 'main'  // replace with your repo URL
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}:latest")
                }
            }
        }

        stage('Docker Login') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-credentials') {
                        // login done automatically by withRegistry
                    }
                }
            }
        }

        stage('Push Image') {
            steps {
                script {
                    docker.image("${IMAGE_NAME}:latest").push()
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                // Apply your kubernetes deployment and service manifests
                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl apply -f service.yaml'
            }
        }
    }
}
