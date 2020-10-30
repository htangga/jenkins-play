pipeline {

  environment {
    registry = 'harbor.unimed.ac.id/library/myweb'
    registryCredential = 'harbor'
    dockerImage = ''
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/htangga/jenkins-play.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }

    stage('Push Image') {
      steps{
        script {
          docker.withRegistry( 'registry' , 'registryCredential' ) {
            dockerImage.push()
          }
        }
      }
    }

    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "unimed-kubeconfig")
        }
      }
    }

  }

}
