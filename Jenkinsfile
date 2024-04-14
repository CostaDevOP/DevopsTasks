pipeline {
    agent any

    stages {
        stage('chekout') {
            steps {
                git url:'https://github.com/CostaDevOP/DevopsTasks.git' , branch: 'main' , credentialsId 'jenkins-exmp-github'
                sh 'ls -la'
            }
        }
    }
}
