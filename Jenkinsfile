pipeline {
    agent any    
    stages {
	stage('Clear Workspace') {
	    steps {
                // Clear the workspace before proceeding
		deleteDir()
            }
        }
        stage('Checkout') {
            steps {
                // Fetch code from GitHub
                git branch: 'main', git 'https://github.com/CostaDevOP/DevopsTasks.git'
            }
        }
    }
}
