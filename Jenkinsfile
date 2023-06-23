pipeline {

  environment {
    dockerimagename = "amaniboussaa/react-app"
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
       git branch: 'main',
      credentialsId: 'github-credentials',
      url: 'https://github.com/amani-boussaa/jenkins-kubernetes-deployment.git'
      }
    }

    stage('Build image') {
      steps{
        script {
      dockerImage = docker.build dockerimagename, "--path-to-docker-executable C:\Program Files\Docker\Docker\resources\bin\docker.exe"
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
          kubernetesDeploy(configs: "deployment.yaml", "service.yaml")
        }
      }
    }

  }

}