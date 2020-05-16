pipeline {

  environment {
    registry = "https://harbor.asaru.info/front-01/myweb"
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/IsuraGateways/test-jenkins.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build -t "https://harbor.asaru.info/front-01/myweb:latest" .
        }
      }
    }

    stage('Push Image') {
      steps{
        script {
            docker.withDockerRegistry(credentialsId: 'harbor', url: 'https://harbor.asaru.info/front-01/testing') {
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
