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
           echo ====+=====================================+====
          '''
        }
      }
    }

    stage('Harbor') {
      steps {
        script {
          // sh 'docker login -u admin -p Harbor12345 harbor.asaru.info'
          // withCredentials([usernameColonPassword(credentialsId: 'harbor', variable: 'HarborCredentilas')]) {
          // some block
          // }
           withCredentials([usernamePassword(credentialsId: 'harbor', passwordVariable: 'p', usernameVariable: 'u')]) {
           sh '''
             docker login -u $u -p $p harbor.asaru.info
             docker tag 192.168.1.81:5000/justme/myweb:$BUILD_NUMBER harbor.asaru.info/front-01/test-netflix:0.0.$BUILD_NUMBER
             
             //docker image rm 192.168.1.81:5000/justme/myweb:$BUILD_NUMBER
             docker images
             '''
           } 
        }
      }
    }

  }

}
