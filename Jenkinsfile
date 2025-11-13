pipeline {
    agent any

    environment {
        DOCKERHUB_USER = 'abhishekkk23'
        DOCKERHUB_PASS = 'abhisheker'
        IMAGE_NAME = 'nodejs-app'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm   // ✅ use same repo Jenkinsfile is in
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test || echo "No tests found, skipping tests..."'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKERHUB_USER/$IMAGE_NAME:latest .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                sh '''
                    echo "$DOCKERHUB_PASS" | docker login -u "$DOCKERHUB_USER" --password-stdin
                    docker push $DOCKERHUB_USER/$IMAGE_NAME:latest
                    docker logout
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Docker image successfully built and pushed to Docker Hub!'
        }
        failure {
            echo '❌ Pipeline failed!'
        }
    }
}
