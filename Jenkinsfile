pipeline {
  environment {
    dockerimagename = "amani-boussaa/react-app2"
    dockerImage = ""
  }

  agent {
    docker {
      image 'docker:latest'
      args '-v /var/run/docker.sock:/var/run/docker.sock'
    }
  }

  stages {
    stage('Checkout Source') {
      steps {
        git branch: 'main',
        credentialsId: 'github-credentials',
        url: 'https://github.com/amani-boussaa/jenkins-kubernetes-deployment.git'
      }
    }
stage('Initialize Docker'){         
	  def dockerHome = tool 'MyDocker'         
	  env.PATH = "${dockerHome}/bin:${env.PATH}"     
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
          docker.withRegistry('https://registry.hub.docker.com', registryCredential) {
            dockerImage.push('latest')
          }
        }
      }
    }

    stage('Deploying React.js container to Kubernetes') {
      steps {
        script {
          kubernetesDeploy(configs: ['deployment.yaml', 'service.yaml'])
        }
      }
    }
  }
}
