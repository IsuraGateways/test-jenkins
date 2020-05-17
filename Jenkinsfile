pipeline {

  environment {
    registry = "chelibane/jsapp"
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
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
          sh '''
           echo ==+==+===+===+==
          '''
        }
      }
    }
    
    stage ("Version Image"){
      steps {
        script {
          sh '''
            docker tag chelibane/jsapp:$BUILD_NUMBER harbor.asaru.info/public-01/test-netflix:0.1.$BUILD_NUMBER
            '''
        }
      }
    }

    stage('Publish Image') {
      steps {
        script {
           withCredentials([usernamePassword(credentialsId: 'harbor', passwordVariable: 'p', usernameVariable: 'u')]) {
           sh '''
             docker login -u $u -p $p harbor.asaru.info
             docker push harbor.asaru.info/public-01/test-netflix:0.1.$BUILD_NUMBER
             '''
           } 
        }
      }
    }
    
    
    stage ("Clean up") {
      steps {
        script {
          sh '''
            echo Cleanning up ...
             docker image rm harbor.asaru.info/public-01/test-netflix:0.1.$BUILD_NUMBER chelibane/jsapp:$BUILD_NUMBER
             docker images
          '''
        }
      }
    }

    
  }
}
