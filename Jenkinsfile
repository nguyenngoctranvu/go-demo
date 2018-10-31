pipeline {
  agent {
    label "docker"
  }
  stages {
    stage('Unit') {
      steps {
        echo 'Uniting...'
        sh "docker-compose -f docker-compose-test.yml run --rm unit"
        sh "docker build -t go-demo ."
      }
    }
    stage('Staging') {
      environment {
        COMPOSE_FILE = 'docker-compose-test-local.yml'
        HOST_IP = 'localhost'
      }
      steps {
        echo 'Staging...'
        sh "docker-compose up -d staging-dep"
        sh "docker-compose run --rm staging"
      }
    }
    stage('Publish') {
      steps {
        echo 'Publishing...'
      }
    }
    stage('Prod-like') {
      steps {
        echo 'Prod-liking...'
      }
    }
    stage('Production') {
      steps {
        echo 'Productioning...'
      }
    }
  }
}
