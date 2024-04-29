# DevOps Tasks
## GitHub > Jenkins > k8s > .netCore

<img src="https://github.com/CostaDevOP/DevopsTasks/blob/main/pipline.png" alt="pipline">

Below are the mission details:
1. Please create a creation process and use it to set up a Jenkins service as a Pod in k8s, in NS called devops and connect it to GIT.
 - There is no meaning to the type of k8s or of GIT.
2. Please create a Jenkins pipeline type process that uses the created Jenkins,
which builds a specific web application in NS, based on .netCore
and then deploys and uploads it as a website on another NS.

## Tools

- [AWS] - as aplatform for K8S (aws eks)
- [terraform]
- [jenkins]
- [Github]
- as .Net app i will use:<br>
  [hello-dotnet] - https://github.com/hseligson1/DockerDemo <br>

## steps
 
1. create AWS user and IAM - costa
2. create Group name - Admin (AdministratorAccess)
3. add S3 (bucket) - saving state
4. create folder terraform:
```
git clone https://github.com/hashicorp/learn-terraform-provision-eks-cluster
```

```
export AWS_ACCESS_KEY_ID=<...><br>
export AWS_SECRET_ACCESS_KEY=<...><br>
```
```
terraform init
```
- correct some settings at terraform.tf (saving state) add :<br>
```
    backend "s3" {
     bucket = "tfs-<...>" 
     key = "devops-terraform-eks"
     region = "eu-central-1"
    }
```
- correct variables.tf for my variable:<br>
```
  variable "region" {
   description = "AWS region"
   type        = string
   default     = "eu-central-1
  }
```
- connect to my eks :<br>
```
   aws eks update-kubeconfig --region <...> --name <...>
```
- get all nodes :<br>
```
kubectl get nodes
```

  ## Setting up Jenkins

  1. using documentation: https://www.jenkins.io/doc/book/installing/kubernetes/
  2. create jenkins folder and :<br>
```
    git clone https://github.com/scriptcamp/kubernetes-jenkins
```
```
    kubectl create namespace devops
```
  - correct all file to my projec (NS , Node name .... )<br>
  - commands i use :<br>
```
kubectl apply -f serviceAccount.yaml
```
     
<br>
    !!! befor create volume we need correct the yaml file: <br>
  ******************************************** <be>

```
        nodeAffinity: 
      required: 
        nodeSelectorTerms: 
        - matchExpressions: 
          - key: kubernetes.io/hostname 
            operator: In 
            values: 
            - <...(kubectl get nodes)> 
```
  
  ******************************************** <br>

``` 
kubectl create -f volume.yaml
``` 

```
kubectl apply -f deployment.yaml
```

  - check deploy status:<br>
```
kubectl get deployments -n devops
```
  - deploy detail:<br>
```
kubectl describe deployments --namespace=devops
```
  - create jenkins service:<br>
``` 
kubectl apply -f service.yaml
```

    - first connect pass command: (kubectl get pods --namespace=devops)<br>
``` 
kubectl exec -it <...> cat /var/jenkins_home/secrets/initialAdminPassword -n devops
```
``` 
kubectl port-forward <...> 32000:32000 -n devops
``` 
  -  $ 403 - [kubectl create clusterrolebinding cluster-system-anonymous --clusterrole=cluster-admin --user=system:anonymous]

 ##### Loggin to Jenkins and config 
 1. install k8s plugins<br>
 2. setting cloud:<br>
    - setting Credentials (.kube/config file)<br>
    - setting pod tamplate<br>
 3. setting Credentials for github (user token from github)
 ##### Github configuration 
 1. crate user token

#### Build docker image for jnlp (costadevop/jnlp-devops-tasks:1) and push to dockerhub
build pipline job :
 name: dotnet-deploying
file: Jenkinsfile

### aws EKS from ver 1.24 not allow us to use a Docker daemon
[https://aws.amazon.com/blogs/containers/all-you-need-to-know-about-moving-to-containerd-on-amazon-eks/]

### as a solution i use kaniko - Build Images In Kubernetes
- Kaniko
    - This tool can be leveraged within a container or Kubernetes cluster.
    - Build and push images in kubernetes cluster.
    - Kaniko can be run as pod/deployment in Kubernetes. Just share the Dockerfile and build artifacts as arguments. The pod builds and pushes the image to specified container registry.
[https://github.com/GoogleContainerTools/kaniko/blob/main/README.md#running-kaniko-in-a-kubernetes-cluster]
<br>



### testing pipeline script
- Create this Secret, naming it regcred:
```
kubectl create secret docker-registry regcred --docker-server=https://index.docker.io/v1/ --docker-username=<name> --docker-password=<pword> --docker-email=<email>
```
```
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
    stage('Build with Kaniko') {
      git branch: 'main', url: 'https://github.com/CostaDevOP/DevopsTasks'
      container('kaniko') {
        sh 'pwd' 
        sh 'ls -la'
        dir('dotNet-Demo') {
            sh "pwd"
            sh '/kaniko/executor -f `pwd`/Dockerfile -c `pwd` --insecure --skip-tls-verify --cache=true --destination=docker.io/costadevop/testpip'
        }
      }
    }
  }
}
```

### !!! if Error : for "executor" Run 'executor --help' for usage
you need to ensure that the IFS is set to null before your executor command
```
sh '''
export IFS=''
/kaniko/executor -f `pwd`/Dockerfile -c `pwd` --insecure --skip-tls-verify --cache=true --destination=docker.io/costadevop/testpip
'''
```



- creating Jenkinsfile for CI/CD pipeline
    -   add new pipeline script from SCM



