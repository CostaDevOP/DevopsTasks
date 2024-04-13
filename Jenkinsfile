pipeline {
    agent any    
    stages {
        stage('Clear Workspace') {
            steps {
                // Clear the workspace before proceeding
                deleteDir()
            }
        }
        stage('Checkout') {
            steps {
                // Fetch code from GitHub
                git branch: 'main', url: 'https://github.com/CostaDevOP/DevopsTasks.git'
            }
        }
        stage('Change Directory') {
            steps {
                // Change to the docker-dotnet-sample folder
                dir('docker-dotnet-sample') {
                    // Commands to execute within the specified directory
                    // For example, you can run shell commands here
                    sh 'ls -la'
                    sh 'pwd'
                    // Add more commands as needed
                }
            }
        }    
        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image using a Dockerfile
                    def customImage = docker.build('costadevop/docker-dotnet-sample')
                }
            }
        }
    }
}
