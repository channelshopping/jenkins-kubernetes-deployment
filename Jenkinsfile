pipeline {
  agent any
  stages {
    stage('Checkout Source') {
      steps {
        git branch: 'main', credentialsId: '3694887a-ed15-43b6-a9be-a3419d37996b', url:'https://github.com/channelshopping/jenkins-kubernetes-deployment.git'
      }
    }

    stage('Build image') {
      steps {
        script {
          dockerImage = docker.build dockerimagename
        }

      }
    }

    stage('Pushing Image') {
      environment {
        registryCredential = 'dockerhub-credentials'
      }
      steps {
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }

      }
    }

    stage('Deploying App to Kubernetes') {
      steps {
        script {
          kubernetesDeploy(configs: "deployment.yaml", "service.yaml")
        }

      }
    }

  }
  environment {
    dockerimagename = 'bravinwasike/react-app'
    dockerImage = ''
  }
}
