pipeline {
    agent any

    environment {
        DOCKERHUB_USER = 'abhishekkk23'
        IMAGE_NAME = 'nodejs-devops-project'
        EC2_HOST = 'ubuntu@15.206.205.65'
        PEM_KEY = 'server1.pem'
    }

    stages {
        stage('Build Docker Image') {
            steps {
                echo '🛠️ Building Docker image...'
                bat """
                docker build -t %DOCKERHUB_USER%/%IMAGE_NAME%:latest .
                """
            }
        }

        stage('Push to Docker Hub') {
            steps {
 echo '🚀 Building Docker image...'
        bat '''
        set PATH=%PATH%;C:\\Program Files\\Docker\\Docker\\resources\\bin
        docker version
        docker build -t abhishekkk23/nodejs-devops-project:latest .
        '''
            }
        }

        stage('Deploy to EC2') {
            steps {
                echo '🌐 Deploying container on EC2...'
                bat """
                pscp -i %PEM_KEY% docker-compose.yml %EC2_HOST%:/home/ubuntu/
                plink -i %PEM_KEY% %EC2_HOST% "docker pull %DOCKERHUB_USER%/%IMAGE_NAME%:latest && docker stop nodeapp || true && docker rm nodeapp || true && docker run -d -p 3000:3000 --name nodeapp %DOCKERHUB_USER%/%IMAGE_NAME%:latest"
                """
            }
        }
    }

    post {
        success {
            echo '✅ Deployment successful!'
        }
        failure {
            echo '❌ Deployment failed. Please check Jenkins logs.'
        }
    }
}
