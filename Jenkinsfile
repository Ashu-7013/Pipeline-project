pipeline {
    agent any
    
    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/your-actual-username/your-actual-repo.git',
                    credentialsId: 'github-credentials'
            }
        }
        // Add more stages as needed
    }
}
