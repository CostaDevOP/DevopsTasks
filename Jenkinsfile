// Uses Declarative syntax to run commands inside a container.
pipeline {
    agent {
        kubernetes {
            // Rather than inline YAML, in a multibranch Pipeline you could use: yamlFile 'jenkins-pod.yaml'
            // Or, to avoid YAML:
            // containerTemplate {
            //     name 'shell'
            //     image 'ubuntu'
            //     command 'sleep'
            //     args 'infinity'
            // }
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
        // stage('build') {
        //     dir('dotNet-Demo') {
        //         sh "docker build -t costadevop/dotnet-demo:$BUILD_NUMBER ."            
        //     }
        // }
        // stage('apply') {
        //     steps {
        //         dir('dotnet-app-yaml') {
        //             withCredentials([file(credentialsId: 'mykubconfigfile', variable: 'MY_FILE')]) {
        //             //sh "cat $MY_FILE"
        //             sh 'mkdir ~/.kube/'        
        //             sh 'cat $MY_FILE > ~/.kube/config'
        //             //sh 'cat ~/.kube/config'
        //             sh 'kubectl apply -f deployment.yaml'
        //             sh 'kubectl apply -f service-lb-dotnet.yaml'
        //             }               
        //         }
        //     }
        // }
    }
}
