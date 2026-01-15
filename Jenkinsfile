pipeline {
  agent any

  environment {
    IMAGE = "stefanobam/devops-web:latest"
    CONTAINER = "devops-web"
    PORT = "8082"
    DOCKERHUB = credentials('dockerhub-creds')
  }

  stages {

    stage('Checkout') {
      steps {
        checkout([$class: 'GitSCM',
          branches: [[name: '*/main']],
          userRemoteConfigs: [[url: 'https://github.com/StefanoCva/devops-docker-jenkins.git']]
        ])
      }
    }

    stage('Build Image') {
      steps {
        sh 'docker build -t $IMAGE .'
      }
    }

    stage('DockerHub Login') {
      steps {
        sh '''
          echo "$DOCKERHUB_PSW" | docker login -u "$DOCKERHUB_USR" --password-stdin
        '''
      }
    }

    stage('Push Image') {
      steps {
        sh 'docker push $IMAGE'
      }
    }
  }

  post {
    success {
      echo "✅ Imagen subida a Docker Hub: $IMAGE"
    }
    failure {
      echo "❌ Falló el pipeline"
    }
  }
}

