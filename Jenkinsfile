pipeline {

  environment {
    dockerimagename = "bhavyascaler/react-app:latest"
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        // Specify branch to avoid ambiguity and ensure the correct source is checked out
        git branch: 'main', url: 'https://github.com/scaler-bhavya/jenkins-docker.git'
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
          docker.withRegistry('https://registry.hub.docker.com', registryCredential) {
            dockerImage.push("latest")
          }
        }
      }
    }

    stage('Deploying React.js container to Kubernetes') {
      steps {
        script {
          // Ensure paths to configurations are correct or relative to the workspace
          kubernetesDeploy(configs: ['./deployment.yaml', './service.yaml'])
        }
      }
    }

  }

}
