pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from GitHub
                git  branch: 'main' ,credentialsId: 'jenkins-exmp-github', url: 'https://github.com/CostaDevOP/DevopsTasks.git'
            }
        }
        stage('Build and Publish Docker Image') {
            steps {
                script {
                    // Build Docker image using the provided Dockerfile
                    docker.build('mywebapp')
                    // Push the built image to a container registry if needed
                    docker.withRegistry('https://yourregistry.com', 'credentialsId') {
                        docker.image('mywebapp').push('latest')
                    }
                }
            }
        }
        // Add more stages as needed, like testing and deployment
    }
}
