pipeline {
    agent any

    environment {
        DOCKER_CMD = "/usr/local/bin/docker"  // Full path to Docker executable (if installed)
        dockerimagename = "bhavyascaler/react-app:latest"
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

        stage('Build Image') {
            steps {
                script {
                    // Build the Docker image using the full path to Docker
                    sh "${DOCKER_CMD} build -t ${dockerimagename} ."
                }
            }
        }

        stage('Push Image (with login)') { // More descriptive stage name
            environment {
                registryCredential = 'dockerhub-credentials' // ID of your Docker Hub credentials
            }
            steps {
                script {
                    // Log in and push the image to Docker Hub
                    docker.withRegistry('https://registry.hub.docker.com', registryCredential) {
                        docker.build(dockerimagename).push() // Use docker.build(...) to get the image object
                    }
                }
            }
        }
    }
}