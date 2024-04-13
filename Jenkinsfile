pipeline {
    agent any

    stages {
        stage('Install .NET SDK') {
            steps {
                script {
                    // Download and install .NET SDK using curl
                    sh 'curl -o dotnet-install.sh https://dot.net/v1/dotnet-install.sh'
                    sh 'chmod +x dotnet-install.sh'
                    sh './dotnet-install.sh -c Current'
                    sh 'export PATH="$HOME/.dotnet:$PATH"'
                }
            }
        }

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
