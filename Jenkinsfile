pipeline {
  environment {
    registry = "docshishir/snake"
    registryCredential = 'training_creds'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        checkout scm
      }
    }
    stage('Building image') {
      steps {
        script {
          dockerImage = docker.build registry
        }
      }
    }
    stage('Deploy Image') {
      steps {
        script {
          docker.withRegistry('https://registry.hub.docker.com', registryCredential) {
            dockerImage.push()
          }
        }
      }
    }
    stage('Pull-image-server') {
      steps {
        sh "docker-compose down"
        sh "docker-compose up -d"
      }
    }
  }
}
