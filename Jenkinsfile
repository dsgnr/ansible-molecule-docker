pipeline {
  environment {
    registry = "dsgnr/ansible-molecule-docker:latest"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }
  agent any
  triggers {
    cron('@daily')
  }
  stages {
    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build registry
        }
      }
    }
    stage('Test image') {
      steps{
        script {
          docker.image(registry).inside(){
            sh '''ansible --version;
                  molecule --version'''
          }
        }
      }
    }

    stage('Push to Dockerhub') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
    stage('Clean image') {
      steps{
        sh "docker rmi $registry"
      }
    }
  }
}
