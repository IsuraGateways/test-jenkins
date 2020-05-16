pipeline {

  environment {
    registry = "192.168.1.81:5000/justme/myweb"
    dockerImage = ""
    dockerImg = ""
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
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
          sh '''
           docker tag  192.168.1.81:5000/justme/myweb:15 harbor.asaru.info/front-01/myweb:latest
          '''
        }
      }
    }

    stage('Push Image') {
      steps{
        script {
            docker.withDockerRegistry(credentialsId: 'harbor', url: 'https://harbor.asaru.info/front-01/testing') {
            dockerImg.push()
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
