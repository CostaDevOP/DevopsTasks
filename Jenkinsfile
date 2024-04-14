pipeline {
    agent any

    environment {
        GIT_CREDENTIALS = 'jenkins-exmp-github'
        GIT_BRANCH = 'main' 
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: "${env.GIT_BRANCH}", credentialsId: "${env.GIT_CREDENTIALS}", url: 'https://github.com/CostaDevOP/DevopsTasks.git'
            }
        }
        stage('Install Docker') {
            steps {
                sh 'curl -fsSL https://get.docker.com -o get-docker.sh'
                sh 'sh get-docker.sh'
                sh 'sudo usermod -aG docker $USER'
                sh 'newgrp docker'
                sh 'docker --version'
            }
        }
        stage('Build and Run') {
            steps {
                sh 'docker --version'
            }
        }
    }
}
