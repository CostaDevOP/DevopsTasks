pipeline {
    agent any

    stages {
        stage('chekout') {
            steps {
                git url:'https://github.com/CostaDevOP/DevopsTasks.git' , branch: 'main' 
                sh 'ls -la'
            }
        }
    }
}
