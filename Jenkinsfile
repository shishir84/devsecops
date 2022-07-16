pipeline {
  agent any
  environment {
    registry = "docshishir/snake"
    registryCredential = 'training_creds'
    app = ''
  }
  stages {
    stage('Cloning Git') {
      steps {
        /* Let's make sure we have the repository cloned to our workspace */
        checkout scm
      }
    }
    stage('Build-and-Tag') {
      steps {
        /* This builds the actual image; synonymous to
         * docker build on the command line */
        app = docker.build registry
      }
    }
    stage('Post-to-dockerhub') {
      steps {
        docker.withRegistry('https://registry.hub.docker.com', registryCredential) {
          app.push("latest")
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
