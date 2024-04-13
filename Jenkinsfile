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
        stage('Build and Run') {
            steps {
                script {
                    dockerComposeBuild()
                    dockerComposeUp()
                }
            }
        }
    }

    post {
        always {
            script {
                dockerComposeDown()
            }
        }
    }
}

def dockerComposeBuild() {
    sh 'docker-compose build'
}

def dockerComposeUp() {
    sh 'docker-compose up -d'
}

def dockerComposeDown() {
    sh 'docker-compose down'
}
