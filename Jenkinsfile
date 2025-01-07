pipeline {
    agent any

    environment {
        DOCKER_CMD = "/usr/local/bin/docker"  // Use full path to Docker binary
        dockerimagename = "bhavyascaler/react-app:latest"
        dockerImage = ''
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
                    // Use the full path to Docker for building the image
                    sh "${DOCKER_CMD} build -t ${dockerimagename} ."
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
                    docker.withRegistry('https://registry.hub.docker.com', registryCredential) {
                        dockerImage.push("latest")
                    }
                }
            }
        }
    }

}
