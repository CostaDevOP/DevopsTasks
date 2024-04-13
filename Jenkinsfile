pipeline {
    agent any

    environment {
        DOTNET_ROOT = '/var/jenkins_home/.dotnet'
        PATH = "$DOTNET_ROOT:$PATH"
        DOTNET_CLI_TELEMETRY_OPTOUT = '1'
    }

    stages {
        stage('Install .NET SDK') {
            steps {
                script {
                    sh 'curl -o dotnet-install.sh https://dot.net/v1/dotnet-install.sh'
                    sh 'chmod +x dotnet-install.sh'
                    sh './dotnet-install.sh -c Current --verbose'
                }
            }
        }

        stage('Verify PATH Configuration') {
            steps {
                sh 'echo $PATH'
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
