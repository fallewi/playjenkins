pipeline {

  environment {
    registry = "192.168.1.250:5000/justme/myweb"
    dockerImage = "fallewi/testkubgit"
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/fallewi/playjenkins.git'
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
          docker.withRegistry( "" ) {
            dockerImage.push()
          }
        }
      }
    }

    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "mykubeconfig")
        }
      }
    }

  }

}
