pipeline {
    agent any

    environment {
             DOCKER_REGISTRY = "docker.io"
              DOCKER_IMAGE = "bhavyascaler/react-app:latest"
            DOCKER_CMD = "/usr/local/bin/docker"  // Full path to Docker executable (if installed)

    }

    stages {
        stage('Install Docker (if needed)') { // More descriptive stage name
            when { // Only run if Docker not found
                expression {
                    !fileExists("/usr/local/bin/docker")
                }
            }
            steps {
                script {
                    echo "Docker not found, installing..."

                    // Check Docker installation
                    sh '/usr/local/bin/docker --version'
                }
            }
        }

        stage('Checkout Source') {
            steps {
                git branch: 'main', url: 'https://github.com/scaler-bhavya/jenkins-docker.git'
            }
        }
        stage('Build Docker Image') {
                  steps {
                      script {
                          docker.build("${DOCKER_IMAGE}:${env.BUILD_ID}")
                      }
                  }
              }

              stage('Push Docker Image') {
                  steps {
                      script {
                          docker.withRegistry('', 'dockerhub-credentials') {
                              docker.image("${DOCKER_IMAGE}:${env.BUILD_ID}").push()
                          }
                      }
                  }
              }


    }
}