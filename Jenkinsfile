pipline {
    agent any
    
    stages {
        stage ("chekout"){
            git url:'https://github.com/CostaDevOP/DevopsTasks.git' , branch: 'main' , credentialsId 'jenkins-exmp-github'
            sh 'ls -la'
        }
    }
}
