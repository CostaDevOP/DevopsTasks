def registry = "costadevop/dotnet-demo"

podTemplate(yaml: '''
              kind: Pod
              spec:
                containers:
                - name: kaniko
                  image: gcr.io/kaniko-project/executor:v1.6.0-debug
                  imagePullPolicy: Always
                  command:
                  - sleep
                  args:
                  - 99d
                  volumeMounts:
                    - name: jenkins-docker-cfg
                      mountPath: /kaniko/.docker
                - name: kubectl
                  image: bitnami/kubectl:latest
                  command:
                    - "/bin/sh"
                    - "-c"
                    - "sleep 99d"
                  tty: true
                  securityContext:
                    runAsUser: 0
                volumes:
                - name: jenkins-docker-cfg
                  projected:
                    sources:
                    - secret:
                        name: regcred
                        items:
                          - key: .dockerconfigjson
                            path: config.json
'''
  ) {
  node(POD_LABEL) {
    stage('Checkout,build,push with Kaniko') {
      git branch: 'main', url: 'https://github.com/CostaDevOP/DevopsTasks'
      sh 'pwd'
      container('kaniko') {
        sh 'pwd' 
        sh 'ls -la'
        dir('dotNet-Demo') {
            sh "pwd"
            sh "/kaniko/executor -f `pwd`/Dockerfile -c `pwd` --insecure --skip-tls-verify --cache=true --destination=docker.io/$registry:$BUILD_NUMBER"
        }
      }
    }
    stage('test'){
        sh 'test Success'
    }
    stage('Deploy to EKS'){
        container(name: 'kubectl', shell: '/bin/sh') {
            withCredentials([file(credentialsId: 'eks-jenkins', variable: 'MY_SEC')]) {
                sh "cat $MY_SEC > ~/.kube/config"
                // sh "cat ~/.kube/config"
                // sh 'apt-get update'
                // sh 'apt-get install -y awscli' 
                // sh 'aws configure list'
                // sh 'aws --version'
                dir('dotnet-app-yaml'){
                    sh "echo $USER"
                    sh 'pwd'
                    // sh "kubectl apply -f deployment.yaml"
                    // sh "kubectl apply -f service-lb-dotnet.yaml"
                }
            }
        }
    }
  }
}
