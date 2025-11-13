pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub')  // Jenkins credentials ID
        DOCKERHUB_REPO = 'abhishekkk23/project-for-devops'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', credentialsId: 'Githublink3', url: 'https://github.com/Abhisheksaini23/Project-for-Devops.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install' // ✅ Use bat instead of sh
            }
        }

        stage('Build Docker Image') {
            steps {
                bat """
                docker build -t %DOCKERHUB_REPO%:latest .
                """
            }
        }

        stage('Login & Push to DockerHub') {
            steps {
                bat """
                docker login -u %DOCKERHUB_CREDENTIALS_USR% -p %DOCKERHUB_CREDENTIALS_PSW%
                docker push %DOCKERHUB_REPO%:latest
                docker logout
                """
            }
        }
    }

    post {
        success {
            echo '✅ Docker image pushed to DockerHub successfully!'
        }
        failure {
            echo '❌ Pipeline failed!'
        }
    }
}
