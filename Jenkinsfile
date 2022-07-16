pipeline {
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        /* Let's make sure we have the repository cloned to our workspace */
        checkout scm
      }
    }
    stage('SAST') {
      steps {
        build 'SECURITY-SAST-SNYK'
      }
    }

    stage('Build-and-Tag') {
      steps {
        /* This builds the actual image; synonymous to
         * docker build on the command line */
        app = docker.build("docshishir/snake")
      }
    }
    stage('Post-to-dockerhub') {
      steps {
        docker.withRegistry('https://registry.hub.docker.com', 'training_creds') {
          app.push("latest")
        }
      }
    }
    stage('SECURITY-IMAGE-SCANNER') {
      steps {
        build 'SECURITY-IMAGE-SCANNER-AQUAMICROSCANNER'
      }
    }

    stage('Pull-image-server') {
      steps {
        sh "docker-compose down"
        sh "docker-compose up -d"
      }
    }
    stage('DAST') {
      steps {
        build 'SECURITY-DAST-OWASP_ZAP'
      }
    }

  }
}
