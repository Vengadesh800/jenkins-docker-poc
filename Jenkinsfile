
pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Fetching source code'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Building Docker image'
                sh 'docker build -t my-devops-app:${BUILD_NUMBER} .'
            }
        }

        stage('Test') {
            steps {
                echo 'Testing Docker image'
                sh 'docker run --rm my-devops-app:${BUILD_NUMBER} nginx -t'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application'

                sh '''
                    docker stop my-devops-container || true
                    docker rm my-devops-container || true

                    docker run -d \
                    --name my-devops-container \
                    --restart unless-stopped \
                    -p 80:80 \
                    my-devops-app:${BUILD_NUMBER}
                '''
            }
        }
    }

    post {
        success {
            echo 'CI/CD Pipeline completed successfully!'
        }

        failure {
            echo 'CI/CD Pipeline failed!'
        }
    }
}
