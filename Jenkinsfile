pipeline {
  environment {
    registry = "hackai87/jenkins01"
    registryCredential = 'hackai87'
    dockerImage = ''
}
agent any
stages {
  stage('Cloning our Git') {
    steps {
      git ([url: 'https://github.com/hackai01/Jenkins', branch: 'main', credentialsId: 'hackai01'])
    }
  }
stage('Building our image') {
  steps{
    script {
      dockerImage = docker.build registry + ":$BUILD_NUMBER"
    }
  }
}
stage('Deploy our image') {
  steps{
    script {
      docker.withRegistry( '', registryCredential ) {
        dockerImage.push()
      }
    }
  }
}
stage('Cleaning up') {
  steps{
    sh "docker rmi $registry:$BUILD_NUMBER"
    }
  }
stage('Build')
{
    steps
    {
    	grypeScan scanDest: 'dir/:tmp', repName: 'myScanResult.txt', autoInstall: true
    }
}
}
}
