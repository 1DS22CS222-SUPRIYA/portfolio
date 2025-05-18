pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'manvisupriya/portfolio-website:latest'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git url: 'https://github.com/1DS22CS222-SUPRIYA/portfolio.git', branch: 'main', credentialsId: 'dockerhub-creds'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t %DOCKER_IMAGE% .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    bat '''
                    echo %PASSWORD% | docker login -u %USERNAME% --password-stdin
                    docker push %DOCKER_IMAGE%
                    '''
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                bat 'kubectl apply -f deployment.yaml'
                bat 'kubectl apply -f service.yaml'
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed.'
        }
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
