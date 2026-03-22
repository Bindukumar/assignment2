pipeline {
    agent { label 'kubernetes' }

    environment {
        DOCKERHUB = credentials('dockerhub')
        IMAGE = "bindukumarm/php-app"
    }

    stages {
        stage('Checkout') {
            steps {
               git credentialsId: 'github', url: 'https://github.com/Bindukumar/assignment2.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t %IMAGE%:%BUILD_NUMBER% .'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                bat 'echo %DOCKERHUB_PSW% | docker login -u %DOCKERHUB_USR% --password-stdin'
            }
        }

        stage('Push Image') {
            steps {
                bat 'docker push %IMAGE%:%BUILD_NUMBER%'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                bat '''
                kubectl set image deployment/php-app php-app=%IMAGE%:%BUILD_NUMBER% --record
                kubectl rollout status deployment/php-app
                '''
            }
        }
    }
}
