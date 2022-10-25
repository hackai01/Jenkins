pipeline {
  environment {
    registry = "hackai87/jenkins"
    registryCredential = 'hackai87'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Clone Git') {
      steps {
        git ([url: 'https://github.com/hackai01/Jenkins', branch: 'main', credentialsId: 'hackai01'])
 
      }
    }
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    stage('Deploy Docker Image') {
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
  }
}
