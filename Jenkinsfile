pipeline {
  agent any
  environment {
    IMAGE_NAME = "demo-ci-cd:latest"
  }
  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }
    stage('Build & Test') {
      steps {
        sh 'mvn -B clean package'
      }
    }
   
  }
  post {
    always {
      junit '**/target/surefire-reports/*.xml'
      archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
    }
  }
}
