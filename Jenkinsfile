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
             docker tag chelibane/jsapp:$BUILD_NUMBER harbor.asaru.info/public-01/test-netflix:0.0.$BUILD_NUMBER
             docker push harbor.asaru.info/public-01/test-netflix:0.0.$BUILD_NUMBER
             docker image rm harbor.asaru.info/public-01/test-netflix:0.0.33 harbor.asaru.info/public-01/test-netflix:0.0.34 harbor.asaru.info/public-01/test-netflix:0.0.35  
             docker image rm 192.168.1.81:5000/justme/myweb:30 192.168.1.81:5000/justme/myweb:31 chelibane/jsapp:32 chelibane/jsapp:33 chelibane/jsapp:34
             docker rm harbor.asaru.info/front-01/test-netflix:0.0.$BUILD_NUMBER chelibane/jsapp:$BUILD_NUMBER
             docker images
             '''
           } 
        }
      }
    }

  }

}
