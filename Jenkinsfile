pipeline {
  environment {
    imagename = "luidoc/flaskapp"
    registryCredential = 'luidoc'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        git([url: 'https://github.com/luidoc/movieapp.git', branch: 'main', credentialsId: 'luidoc'])

      }
    }
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build imagename
        }
      }
    }
    stage('Deploy Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push("$BUILD_NUMBER")
             dockerImage.push('latest')
          }
        }
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $imagename:$BUILD_NUMBER"
         sh "docker rmi $imagename:latest"

      }
    }
    stage('Deploy image on remote docker') {
      steps{
        sh "docker context use node2"
        sh "docker run -d --name flaskapp $imagename:latest"
      }
    }
  }
}
