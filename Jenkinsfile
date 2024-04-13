pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/CostaDevOP/DevopsTasks.git'
            }
        }

        stage('Build .NET App') {
            steps {
                // You may need to adjust these commands based on your project structure
                sh 'cd docker-dotnet-sample && dotnet restore && dotnet build'
            }
        }

        stage('Deploy with Docker Compose') {
            steps {
                sh 'cd docker-dotnet-sample && docker-compose up -d'
            }
        }
    }
}
