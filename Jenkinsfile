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
    stage('Build') {
      steps {
        echo 'Building...'
      }
    }
    stage('Staging') {
      steps {
        echo 'Staging...'
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
