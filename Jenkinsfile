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


pipeline {
    agent any

    environment {
        GIT_CREDENTIALS = 'jenkins-exmp-github'
        GIT_BRANCH = 'main' // Change this to your desired branch
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from GitHub
                git branch: "${env.GIT_BRANCH}", credentialsId: "${env.GIT_CREDENTIALS}", url: 'https://github.com/CostaDevOP/DevopsTasks.git'
            }
        }
        stage('Build and Run') {
            steps {
                // Run docker-compose up
                script {
                    dockerComposeBuild()
                    dockerComposeUp()
                }
            }
        }
    }

    post {
        always {
            // Clean up after the build
            script {
                dockerComposeDown()
            }
        }
    }
}

def dockerComposeBuild() {
    // Build docker images if needed
    sh 'docker-compose build'
}

def dockerComposeUp() {
    // Start containers
    sh 'docker-compose up -d'
}

def dockerComposeDown() {
    // Stop and remove containers
    sh 'docker-compose down'
}
