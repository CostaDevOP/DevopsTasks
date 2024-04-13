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
        stage('Install and Initialize Docker') {
            steps {
                // Check if Docker is installed
                script {
                    def dockerInstalled = sh(script: 'docker --version', returnStatus: true) == 0
                    if (!dockerInstalled) {
                        // Install Docker
                        sh 'curl -fsSL https://get.docker.com -o get-docker.sh'
                        sh 'sudo sh get-docker.sh'
                        // Add Jenkins user to Docker group to avoid sudo requirement
                        sh 'sudo usermod -aG docker jenkins'
                        // Start Docker service
                        sh 'sudo systemctl start docker'
                    }
                }
                // Initialize Docker if needed
                sh 'docker --version'
                // Add any other Docker initialization steps here
            }
        }
    }
}
