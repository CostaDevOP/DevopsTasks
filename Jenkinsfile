pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git credentialsId: 'jenkins-exmp-github', url: 'https://github.com/CostaDevOP/DevopsTasks.git', branch: 'main'
            }
        }

        stage('Build .NET App') {
            steps {
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
