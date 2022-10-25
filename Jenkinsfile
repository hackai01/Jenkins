pipeline {
  environment {
    registry = "hackai87/jenkins"
    registryCredential = 'hackai87'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Cloning Git') {
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
    stage ('Scan') {
      steps {
        sh 'apk add bash curl'
        sh 'curl -s https://ci-tools.anchore.io/inline_scan_latest | bash -s -- -d Dockerfile -b .anchore_policy.json ${IMAGE_NAME}:ci'
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
        sh "docker rmi $registry:$BUILD_NUMBER"

      }
    }
  }
}
