pipeline {
  agent {
    label "docker"
  }
  stages {
    stage('Unit') {
      steps {
        echo 'Uniting...'
        sh "whoami"
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
        sh "docker tag go-demo hub.fimplus.io/repo/go-demo:2.${BUILD_NUMBER}"
        sh "docker login -u $USER -p $PASS hub.fimplus.io"
        sh "docker push hub.fimplus.io/repo/go-demo:2.${BUILD_NUMBER}"
        sh "docker rmi hub.fimplus.io/repo/go-demo:2.${BUILD_NUMBER}"
      }
    }
    stage('Prod-like') {
      environment {
        HOST_IP = "10.10.200.74"

      }
      steps {
        echo 'Prod-liking...'
        script {
          try {
            sh "docker service update --image hub.fimplus.io/repo/go-demo:2.${env.BUILD_NUMBER} go-demo_main"
            for (i=0; i<3 ; i++) {
              sh "curl -I $HOST_IP/demo/hello"
            }
          } catch (e) {
            sh "Deploy Prod-like failed"
          }
        }
      }
    }
    stage('Production') {
      environment {
        HOST_IP = "localhost"
        COMPOSE_FILE = 'docker-compose-test-local.yml'
      }
      steps {
        echo 'Productioning...'
        script {
          try {
            sh "docker service update --image localhost:5000/go-demo:2.${env.BUILD_NUMBER} go-demo_main"
            // sh "docker-compose run --rm production"
          } catch (e) {
            // sh "docker service rollback go-demo_main"
            echo "deploy production failed"
          }
        }
      }
    }
  }
  post {
    always {
      sh "docker-compose -f docker-compose-test-local.yml down"
    }
  }
}
