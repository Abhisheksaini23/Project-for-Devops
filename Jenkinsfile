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
                bat '''
        set PATH=%PATH%;C:\\Program Files\\Docker\\Docker\\resources\\bin
        docker --version
        docker build -t abhishekkk23/project-for-devops:latest .
        '''
            }
        }

        stage('Login & Push to DockerHub') {
            steps {
               bat '''
        set PATH=C:\\Program Files\\Docker\\Docker\\resources\\bin;%PATH%
        docker --version
        docker login -u abhishekkk23 -p abhisheker
        docker push abhishekkk23/project-for-devops:latest
        docker logout
        '''
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
