pipeline {
  agent any
  environment {
    COMPOSE_FILE = 'docker-compose-test-local.yml'
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
        script {
          try {
            sh 'docker-compose up -d staging-dep'
            sh 'docker-compose run --rm staging'
          } catch(e) {
            error "Staging failled"
          } finally {
            sh 'docker-compose down'
          }
        }
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
  post {
    always {
      sh 'docker-compose down'
    }
  }
}
