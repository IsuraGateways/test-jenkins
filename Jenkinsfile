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
           docker images 
           echo ====+=====================================+=====
           docker ps -a
           docker image rm d9c91ee0763a
           
          '''
        }
      }
    }


    stage('Harbor') {
      steps {
        script {
          sh 'docker login -u admin -p Harbor12345 harbor.asaru.info'
        }
      }
    }

  }

}
