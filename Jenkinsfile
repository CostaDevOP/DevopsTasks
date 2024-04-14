
pipeline {
    agent {
        kubernetes {
            yaml '''
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: jnlp-devops
    image: costadevop/jnlp-devops-tasks:1.1  
    command:
    - sleep
    args:
    - infinity
'''
            // Can also wrap individual steps:
            // container('shell') {
            //     sh 'hostname'
            // }
            defaultContainer 'jnlp-devops'
        }
    }
    stages {
        stage('checkout') {
            steps {
                git url:'https://github.com/CostaDevOP/DevopsTasks.git' , branch: 'main'
                sh 'pwd'
            }
        }
        stage('apply') {
            steps {
                dir('dotnet-app-yaml') {
                    withCredentials([file(credentialsId: 'mykubconfigfile', variable: 'MY_FILE')]) {
                    sh 'kubectl apply -f deployment.yaml'
                    sh 'kubectl apply -f service-lb-dotnet.yaml'
                    }               
                }
            }
        }
    }
}
