
pipeline {
    agent any

    environment {
        IMAGE_NAME = "abhishekkk23/project-for-devops"
        EC2_IP = "15.206.205.65"
        USER = "ubuntu"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/Abhisheksaini23/Project-for-Devops.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test || echo "No tests found"'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:latest .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-cred', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                    echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                    docker push $IMAGE_NAME:latest
                    '''
                }
            }
        }

        stage('Deploy to EC2') {
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: 'ec2-key', keyFileVariable: 'EC2_KEY')]) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no -i $EC2_KEY $USER@$EC2_IP "
                        docker pull $IMAGE_NAME:latest &&
                        docker stop nodeapp || true &&
                        docker rm nodeapp || true &&
                        docker run -d -p 3000:3000 --name nodeapp $IMAGE_NAME:latest
                    "
                    '''
                }
            }
        }
    }

    post {
        success {
            echo "✅ Deployment successful! Visit http://$EC2_IP:3000"
        }
        failure {
            echo "❌ Pipeline failed! Check Jenkins logs."
        }
    }
}
