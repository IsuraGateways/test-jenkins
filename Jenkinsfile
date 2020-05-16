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
          
           docker image rm 192.168.1.81:5000/justme/myweb:18 image rm 192.168.1.81:5000/justme/myweb:21 image rm 192.168.1.81:5000/justme/myweb:22 image rm 192.168.1.81:5000/justme/myweb:23
           docker images 
           
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
