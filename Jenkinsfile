pipeline {
    agent any

    environment {
        dockerimagename = "bhavyascaler/react-app:latest"
        // Use a variable to store the Docker image object
        dockerImage = ''
        // Correct PATH to ensure Docker can be accessed
        PATH = "/usr/local/bin:$PATH"  // Add /usr/local/bin to PATH to ensure Docker command is found
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
                    // Build the Docker image using the correct Docker command
                    dockerImage = docker.build(dockerimagename)
                }
            }
        }

        stage('Push Image') {
            environment {
                // It's assumed 'dockerhub-credentials' is an ID in Jenkins Credential Store
                registryCredential = 'dockerhub-credentials'
            }
            steps {
                script {
                    // Log in and push the image to Docker Hub
                    docker.withRegistry('https://registry.hub.docker.com', registryCredential) {
                        dockerImage.push("latest")
                    }
                }
            }
        }
    }

}
