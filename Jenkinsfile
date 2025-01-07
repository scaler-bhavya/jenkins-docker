pipeline {
    agent any

    environment {
        DOCKER_CMD = "/usr/local/bin/docker"  // Full path to Docker executable (if installed)
        dockerimagename = "bhavyascaler/react-app:latest"
        dockerImage = ''
    }

    stages {
        stage('Install Docker') {
            steps {
                script {
                    // Install Docker if it's not installed
                    if (!fileExists("/usr/local/bin/docker")) {
                        echo "Docker not found, installing..."
                        sh '''
                        # Update the package manager and install dependencies
                        sudo apt-get update
                        sudo apt-get install -y apt-transport-https ca-certificates curl software-properties-common

                        # Add Docker's official GPG key and repository
                        curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
                        sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

                        # Install Docker
                        sudo apt-get update
                        sudo apt-get install -y docker-ce

                        # Ensure Docker is running
                        sudo systemctl start docker
                        sudo systemctl enable docker
                        '''

                        // Check Docker installation
                        sh '/usr/local/bin/docker --version'
                    } else {
                        echo "Docker is already installed."
                    }
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
                    sh '${DOCKER_CMD} build -t ${dockerimagename} .'
                    dockerImage = docker.build(dockerimagename)
                }
            }
        }

        stage('Push Image') {
            environment {
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
