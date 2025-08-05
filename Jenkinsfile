pipline {
  agent any

  environment {
    DOCKERHUB_USER = credentials('docker_username')
    DOCKERHUB_PASS = credentials('docker_password')
  }
  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }
  }
}
