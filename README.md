# DevOps Tasks
## K8S > Jenkins > .netCore

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
- as dotNet app i will use:<br>
  [hello-dotnet] - https://github.com/dotnet/dotnet-docker/blob/main/samples/kubernetes/hello-dotnet <br>

## steps
 
1. create AWS user and IAM - costa
2. create Group name - Admin (AdministratorAccess)
3. add S3 (bucket) - saving state
4. create folder terraform:
- $ git clone https://github.com/hashicorp/learn-terraform-provision-eks-cluster<br>
- terraform init<br>
    export AWS_ACCESS_KEY_ID=<...><br>
    export AWS_SECRET_ACCESS_KEY=<...><br>
- correct some settings at terraform.tf (saving state) add :<br>
    backend "s3" {<br>
     bucket = "tfs-<...>" <br>
     key = "devops-terraform-eks"<br>
     region = "eu-central-1"<br>
    }<br>
- correct variables.tf for my variable:<br>
  variable "region" {<br>
   description = "AWS region"<br>
   type        = string<br>
   default     = "eu-central-1<br>
  }<br>
- connect to my eks :<br>
   $ aws eks update-kubeconfig --region <...> --name <...><br>
- get all nodes :<br>
   $ kubectl get nodes<br>

  ## Setting up Jenkins

  1. using documentation: https://www.jenkins.io/doc/book/installing/kubernetes/
  2. create jenkins folder and :<br>
      $ git clone https://github.com/scriptcamp/kubernetes-jenkins<br>
      $ kubectl create namespace devops<br>
  - correct all file to my projec (NS , Node name .... )<br>
  - commands i use :<br>
     $ kubectl apply -f serviceAccount.yaml<br>
     $ kubectl create -f volume.yaml<br>
     $ kubectl apply -f deployment.yaml<br>
  - check deploy status:<br>
     $ kubectl get deployments -n devops<br>
  - deploy detail:<br>
     $ kubectl describe deployments --namespace=devops<br>
  - create jenkins service:<br>
     $ kubectl apply -f service.yaml<br>

  - first connect pass command: (kubectl get pods --namespace=devops)<br>
     $ kubectl exec -it <...> cat /var/jenkins_home/secrets/initialAdminPassword -n devops<br>

  
    
    
