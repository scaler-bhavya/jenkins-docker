pipeline {
    agent any

    environment {
      DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials')
    }

    stages {
        stage('Checkout Source') {
            steps {
                git branch: 'main', url: 'https://github.com/scaler-bhavya/jenkins-docker.git'
            }
        }

        stage('Build Image') {
            steps {
                script {
                    // Build the Docker image using the full path to Docker
                    sh 'docker build -t bhavyascaler/react-app:latest .'
                }
            }
        }
        stage('Login') {
            steps {
             sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
          }
      }
       stage('Push') {
      steps {
        sh 'docker push bhavyascaler/react-app:latest'
      }
    }
    stage('Deploy to Kubernetes') {
            steps {
                kubernetesDeploy(
                    configs: 'deployment.yaml',
                    kubeconfigId: 'k8s-credentials'
                )
            }
        }
    }
}