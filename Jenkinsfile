pipeline {
  agent any
  stages {
    stage('Clone repository') {
      steps {
        checkout scm
      }
    }

    stage('Build image') {
      steps {
        script {
          // Define app variable here
          def app = docker.build("35311/kiii-jenkins", "-f Dockerfile .")
          // Set the app variable as an environment variable
          env.APP = app
        }
      }
    }

    stage('Push image') {
      steps {
        script {
          // Retrieve the app variable from environment
          def app = env.APP
          docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            app.push("${env.BRANCH_NAME}-${env.BUILD_NUMBER}")
            app.push("${env.BRANCH_NAME}-latest")
            // signal the orchestrator that there is a new version
          }
        }
      }
    }
  }
}
