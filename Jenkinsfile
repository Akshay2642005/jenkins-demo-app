pipeline {
  agent any

  environment {
    DOCKERHUB_USER = credentials('dockerhub_username')
    DOCKERHUB_PASS = credentials('dockerhub_password')
  }

  stages {
    stage('Init') {
      steps {
        script {
          env.IMAGE = "${env.DOCKERHUB_USER}/vite-app"
        }
      }
    }

    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Install') {
      steps {
        sh 'npm ci'
      }
    }

    stage('Test') {
      steps {
        sh '''
          echo " Running unit tests..."
          npm run test

          echo " Checking for vulnerabilities..."
          npm audit --production || echo " Vulnerabilities found."

          echo " Ensuring dist folder builds..."
          npm run build
          test -d dist || { echo "dist/ folder missing!"; exit 1; }
        '''
      }
    }

    stage('Docker Build & Push') {
      steps {
        sh '''
          echo $DOCKERHUB_PASS | docker login -u $DOCKERHUB_USER --password-stdin
          docker build -t $IMAGE:latest .
          docker push $IMAGE:latest
        '''
      }
    }
  }

  post {
    success {
      echo ' Build, test, and deployment successful!'
    }
    failure {
      echo ' Build failed. Check logs above.'
    }
  }
}


