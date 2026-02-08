pipeline {
  agent any

  stages {

    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build Docker Image') {
      steps {
        sh 'docker build -t vanessa251/ci-cd-web .'
      }
    }

    stage('Push Image') {
      steps {
        withCredentials([string(credentialsId: 'docker_hub', variable: 'DOCKER_PASS')]) {
          sh '''
          docker login -u vanessa251 -p $DOCKER_PASS
          docker push vanessa251/ci-cd-web
          '''
        }
      }
    }

    stage('Deploy to Kubernetes') {
      steps {
        kubernetesDeploy configs: 'deploymentsvc.yaml', kubeconfigId: 'kubernetes_config'
      }
    }
  }
}
