pipeline {
  agent any
  stages {
    stage('Pull') {
      steps {
        sh 'echo "Hello World"'
      }
    }
    stage('Unit') {
      steps {
        sh 'echo Build'
      }
    }
    stage('Staging') {
      steps {
        sh 'echo Staging'
      }
    }
    stage('Publish') {
      steps {
        sh 'echo Publish'
      }
    }
    stage('Prod-like') {
      steps {
        sh 'echo Prod-like'
      }
    }
    stage('Prod') {
      steps {
        sh "echo Prod"
      }
    }
  }

}
