pipeline {

  environment {
    dockerimagename = "vishponnuru/react-app"
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git branch: 'main', credentialsId: 'github-credentials', url:'https://github.com/vponnuru23/jenkins-k8s-deployment.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }

    stage('Pushing Image') {
      environment {
               registryCredential = 'dockerhub-credentials'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }

    stage('Deploying React.js container to Kubernetes') {
      steps {
        script {
          sh 'envsubst < ${WORKSPACE}/deployment.yaml | kubectl apply -f -'
          sh 'envsubst < ${WORKSPACE}/service.yaml | kubectl apply -f -'
        }
      }
    }

  }

}
