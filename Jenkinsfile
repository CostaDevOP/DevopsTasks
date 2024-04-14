podTemplate(label: 'mypod', containers: [
    containerTemplate(name: 'jnlp', image: 'jenkins/jnlp-slave:latest', args: ['\$(JENKINS_SECRET)', '\$(JENKINS_NAME)'], privileged: true),
    containerTemplate(name: 'tools', image: 'your-image-with-sudo:latest', command: 'cat', ttyEnabled: true)
  ],
  volumes: [
    hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock')
  ]) {
  node('mypod') {
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
        stage('Install Docker') {
            steps {
                sh 'sudo apt update'
                sh 'sudo apt upgrade'
                sh 'curl -fsSL https://get.docker.com -o get-docker.sh'
                sh 'sh get-docker.sh'
                sh 'sudo usermod -aG docker $USER'
                sh 'newgrp docker'
                sh 'docker --version'
            }
        }
        stage('Build and Run') {
            steps {
                sh 'docker --version'
            }
        }
    }
  }
}
