pipeline {
  agent any
  environment {
    COMPOSE_FILE = docker-compose-test-local.yml
  }
  stages {
    stage('Unit') {
      steps {
        sh 'docker-compose run --rm unit'
        sh 'docker-compose build app'
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
