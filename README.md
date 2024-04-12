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

## steps
 
1. create AWS user and IAM - costa
2. create Group name - Admin (AdministratorAccess)
3. add S3 (bucket) - saving state
4. create folder terraform:
- git clone https://github.com/hashicorp/learn-terraform-provision-eks-cluster
- terraform init
-   export AWS_ACCESS_KEY_ID=<>
    export AWS_SECRET_ACCESS_KEY=<>
- correct some settings at terraform.tf (saving state) add :
      backend "s3" {
    bucket = "tfs-<>" 
    key = "learn-terraform-eks"
    region = "eu-central-1"
    }
- correct variables.tf for my variable:
    variable "region" {
  description = "AWS region"
  type        = string
  default     = "eu-central-1"
}

