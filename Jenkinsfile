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
                git 'https://github.com/your-username/your-repo.git'
            }
        }
    }
}
