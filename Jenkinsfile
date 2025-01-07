pipeline {
    agent any

    environment {
        DOCKER_CMD = "/usr/local/bin/docker"  // Full path to Docker executable (if installed)
        dockerimagename = "bhavyascaler/react-app:latest"
        registryCredential = 'dockerhub-credentials'  // Define the registry credential ID
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
                    sh "${DOCKER_CMD} build -t ${dockerimagename} ."
                }
            }
        }

        stage('Push Image') {
            steps {
                script {
                    // Push the Docker image to a custom registry (Docker Hub in this case)
                    docker.withRegistry('https://registry.hub.docker.com', registryCredential) {
                        // Build and push the image
                        def dockerImage = docker.image(dockerimagename)
                        dockerImage.push("latest")  // Push the image to Docker Hub
                    }
                }
            }
        }
    }
}
