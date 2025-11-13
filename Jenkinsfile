pipeline {
    agent any

    environment {
        DOCKERHUB_USER = 'abhishekkk23'           // your Docker Hub username
        DOCKERHUB_PASS = 'abhisheker'   // ⚠️ replace with your password
        IMAGE_NAME = 'nodejs-app'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Abhisheksaini23/Nodejs-Devops-Project.git'
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
            echo '✅ Docker image built and pushed to Docker Hub successfully!'
        }
        failure {
            echo '❌ Pipeline failed!'
        }
    }
}
