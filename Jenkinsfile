pipeline {
    agent any    
    agent {
        kubernetes {
            // Define the Pod template
            yaml """
            apiVersion: v1
            kind: Pod
            metadata:
              labels:
                app: kubepods
            spec:
              containers:
              - name: dotnet
                image: mcr.microsoft.com/dotnet/core/sdk:3.1
                command: ['sleep', 'infinity']
            """
        }
    }
    stages {
        stage('Clear Workspace') {
        stage('Pull Source Code') {
            steps {
                // Clear the workspace before proceeding
                deleteDir()
                // Clone the repository from GitHub
                git branch: 'main', credentialsId: 'your-credentials-id', url: 'https://github.com/CostaDevOP/DevopsTasks.git'
            }
        }
        stage('Checkout') {
        stage('Build') {
            steps {
                // Fetch code from GitHub
                git branch: 'main', url: 'https://github.com/CostaDevOP/DevopsTasks.git'
                container('dotnet') {
                    // Run dotnet build command
                    sh 'dotnet build'
                }
            }
        }  
    }
