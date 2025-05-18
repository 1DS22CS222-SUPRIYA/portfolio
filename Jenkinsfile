pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Checkout your GitHub repo
                git url: 'https://github.com/1DS22CS222-SUPRIYA/portfolio.git', credentialsId: 'github-token', branch: 'main'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image with your Docker Hub repo name and tag
                    bat 'docker build -t manvisupriya/portfolio:latest .'
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKERHUB_USERNAME', passwordVariable: 'DOCKERHUB_PASSWORD')]) {
                    bat '''
                        docker logout
                        docker login -u %DOCKERHUB_USERNAME% -p %DOCKERHUB_PASSWORD%
                    '''
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                bat 'docker push manvisupriya/portfolio:latest'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                // Add your kubectl commands here for deployment
                // Example:
                bat 'kubectl apply -f k8s-deployment.yaml'
            }
        }
    }
}
