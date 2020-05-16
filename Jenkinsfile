pipeline {

  environment {
    registry = "chelibane/jsapp"
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
           withCredentials([usernamePassword(credentialsId: 'harbor', passwordVariable: 'p', usernameVariable: 'u')]) {
           sh '''
             docker login -u $u -p $p harbor.asaru.info
             docker images
             docker tag chelibane/jsapp:$BUILD_NUMBER harbor.asaru.info/public-01/test-netflix:0.0.$BUILD_NUMBER
             docker push harbor.asaru.info/public-01/test-netflix:0.0.$BUILD_NUMBER
             
             docker image rm chelibane/jsapp:36 chelibane/jsapp:37 chelibane/jsapp:38 chelibane/jsapp:35 harbor.asaru.info/public-01/test-netflix:0.0.36  harbor.asaru.info/public-01/test-netflix:0.0.37 
             
             docker image rm harbor.asaru.info/public-01/test-netflix:0.0.$BUILD_NUMBER chelibane/jsapp:$BUILD_NUMBER
             '''
           } 
        }
      }
    }

  }

}
