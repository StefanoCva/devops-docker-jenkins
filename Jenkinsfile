pipeline {
  agent any

  environment {
    IMAGE = "devops-web:latest"
    CONTAINER = "devops-web"
    PORT = "8082"
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

    stage('Deploy (Run Container)') {
      steps {
        sh '''
          docker rm -f $CONTAINER || true
          docker run -d --name $CONTAINER -p ${PORT}:80 $IMAGE
          docker ps --filter "name=$CONTAINER"
        '''
      }
    }

    stage('Smoke Test') {
      steps {
        sh '''
          echo "Probando que responda HTTP..."
          curl -I --max-time 10 http://localhost:${PORT} | head -n 1
        '''
      }
    }
  }

  post {
    always {
      echo "Pipeline finalizado."
    }
  }
}

