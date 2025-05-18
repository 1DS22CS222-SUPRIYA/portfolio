pipeline {
    agent any

    stages {
        stage('Clone repository') {
            steps {
                git url: 'https://github.com/1DS22CS222-SUPRIYA/portfolio.git', branch: 'main'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t supriya/portfolio .'
            }
        }

        stage('Run Docker Container') {
    steps {
        bat '''
        docker rm -f portfolio || echo "No existing container to remove"
        docker run -d -p 3000:3000 --name portfolio supriya/portfolio
        '''
    }
}


        stage('Push to Docker Hub') {
    steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
            bat '''
            docker login -u %USERNAME% -p %PASSWORD%
            docker push supriya/portfolio:latest
            '''
        }
    }
}

        stage('Clean Up') {
            steps {
                bat 'docker stop portfolio'
                bat 'docker rm portfolio'
                bat 'docker rmi supriya/portfolio'
            }
        }
    }
}
