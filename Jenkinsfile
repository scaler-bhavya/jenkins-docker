pipeline {
    agent any

    environment {
        dockerimagename = "bhavyascaler/react-app:latest"
        // Use a variable to store the Docker image object
        dockerImage = ''
        // Setting the PATH to ensure Docker can be accessed
        PATH = "/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/opt/homebrew/bin:$PATH"
    }

    stages {
        stage('Checkout Source') {
            steps {
                git branch: 'main', url: 'https://github.com/scaler-bhavya/jenkins-docker.git'
            }
        }

        stage('Build image') {
            steps {
                script {
                    // Build the Docker image from the Dockerfile located at the root of the project
                    dockerImage = docker.build(dockerimagename)
                }
            }
        }

        stage('Pushing Image') {
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

        stage('Deploying React.js container to Kubernetes') {
            steps {
                script {
                    // Deploy to Kubernetes using the configurations in 'deployment.yaml' and 'service.yaml'
                    kubernetesDeploy(configs: ['deployment.yaml', 'service.yaml'])
                }
            }
        }
    }

    post {
        always {
            // Cleanup action to ensure Docker logs out from the registry
            echo 'Cleaning up post build...'
            sh 'docker logout'
        }
    }
}
