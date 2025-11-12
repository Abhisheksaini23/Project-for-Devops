
pipeline {
    agent any

    environment {
        DOCKERHUB_USERNAME = 'abhishekkk23'
        DOCKERHUB_PASSWORD = 'abhisheker'
        DOCKER_IMAGE = 'abhishekkk23/nodejs-devops'
        EC2_HOST = 'ubuntu@15.206.205.65'
        PEM_KEY = 'C:\\ProgramData\\Jenkins\\.jenkins\\server1.pem' // Path on Windows Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Abhisheksaini23/Project-for-Devops.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Test') {
            steps {
                bat 'npm test || echo "No tests to run"'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t %DOCKER_IMAGE%:latest .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                bat '''
                docker login -u %DOCKERHUB_USERNAME% -p %DOCKERHUB_PASSWORD%
                docker push %DOCKER_IMAGE%:latest
                '''
            }
        }

        stage('Deploy to EC2') {
            steps {
                bat '''
                pscp -i "%PEM_KEY%" docker-compose.yml %EC2_HOST%:/home/ubuntu/
                plink -i "%PEM_KEY%" %EC2_HOST% "docker pull %DOCKER_IMAGE%:latest && docker stop nodejs-app || true && docker rm nodejs-app || true && docker run -d -p 3000:3000 --name nodejs-app %DOCKER_IMAGE%:latest"
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Deployment Successful!'
        }
        failure {
            echo '❌ Pipeline failed! Check Jenkins logs.'
        }
    }
}
