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
            sh "docker-compose down"
          }
        }
      }
    }
    stage('Publish') {
      steps {
        sh "docker tag go-demo localhost:5000/go-demo:${env.BUILD_NUMBER}"
        sh "docker push localhost:5000/go-demo:${env.BUILD_NUMBER}"
      }
    }
    stage('Prod-like') {
      steps {
        sh "docker service update --image localhost:5000/go-demo:${env.BUILD_NUMBER} go-demo_main"
      }
    }
  }
}
