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
        sh "docker tag go-demo localhost:5000/go-demo:2.${BUILD_NUMBER}"
        sh "docker push localhost:5000/go-demo:2.${BUILD_NUMBER}"
        sh "docker rmi localhost:5000/go-demo:2.${BUILD_NUMBER}"
      }
    }
    stage('Prod-like') {
      environment {
        DOCKER_HOST = "tcp://${env.PROD_LIKE_IP}:2376"
        DOCKER_CERT_PATH = "/machines/${env.PROD_LIKE_NAME}"
        DOCKER_TLS_VERIFY = "1"
        HOST_IP = "localhost"
        COMPOSE_FILE = 'docker-compose-test-local.yml'
      }
      steps {
        echo 'Prod-liking...'
        script {
          try {
            sh "docker service update --image localhost:5000/go-demo:2.${env.BUILD_NUMBER} go-demo_main"
            sh "docker-compose run --rm production"
          } catch (No) {
            sh "docker service rollback go-demo_main"
          }
        }
      }
    }
    stage('Production') {
      environment {
        DOCKER_HOST = "tcp://${env.PROD_IP}:2376"
        DOCKER_CERT_PATH = "/machines/${env.PROD_NAME}"
        DOCKER_TLS_VERIFY = "1"
        HOST_IP = "localhost"
        COMPOSE_FILE = 'docker-compose-test-local.yml'
      }
      steps {
        echo 'Productioning...'
        script {
          try {
            sh "docker service update --image localhost:5000/go-demo:2.${env.BUILD_NUMBER} go-demo_main"
            sh "docker-compose run --rm production"
          } catch (e) {
            sh "docker service rollback go-demo_main"
          }
        }
      }
    }
  }
}
