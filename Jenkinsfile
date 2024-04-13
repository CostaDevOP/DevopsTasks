pipeline {
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
                ports:
                    - containerPort: 5000
            """
            // Define namespace
            defaultContainer 'jnlp'
            namespace 'devops'
        }
    }
    stages {
        stage('Pull Source Code') {
            steps {
                // Clone the repository from GitHub
                git branch: 'main', credentialsId: 'jenkins-exmp-github', url: 'https://github.com/CostaDevOP/DevopsTasks.git'
            }
        }
        stage('Build') {
            steps {
                container('dotnet') {
                    // Run dotnet build command
                    sh 'dotnet build'
                }
            }
        }
        stage('Test') {
            steps {
                container('dotnet') {
                    // Run dotnet test command
                    sh 'dotnet test'
                }
            }
        }
        stage('Deploy') {
            steps {
                container('dotnet') {
                    // Run dotnet publish command
                    sh 'dotnet publish -c Release -o ./publish'
                }
                // Deploy the published application to Kubernetes
                kubernetesDeploy (
                    configs: 'deployment.yaml',
                    kubeconfigId: 'kubeconfig',
                    enableConfigSubstitution: true
                )
            }
        }
    }
}
