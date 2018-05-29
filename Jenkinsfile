pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh 'docker build -t demo-image-develop .'
        sh 'ls -latr'
      }
    }
  }
}