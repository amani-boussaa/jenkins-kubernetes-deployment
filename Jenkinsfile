pipeline {
  environment {
    dockerimagename = "amani-boussaa/react-app2"
    dockerImage = ""
  }

  agent { node { label 'docker' } }

  stages {
    stage('Checkout Source') {
      steps {
        git branch: 'main',
        credentialsId: 'github-credentials',
        url: 'https://github.com/amani-boussaa/jenkins-kubernetes-deployment.git'
      }
    }

    
   
    stage('Build image') {
      steps {
       sh 'docker build -t amani-boussaa/react-app:latest .'
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
