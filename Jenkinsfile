pipeline {
    agent any

    environment {
        IMAGE_NAME = "devops-web"
        IMAGE_TAG  = "latest"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/StefanoCva/devops-docker-jenkins.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t $IMAGE_NAME:$IMAGE_TAG .
                '''
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                docker rm -f devops-web || true
                docker run -d -p 8082:80 --name devops-web $IMAGE_NAME:$IMAGE_TAG
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline ejecutado correctamente'
        }
        failure {
            echo '❌ Fallo en el pipeline'
        }
    }
}
