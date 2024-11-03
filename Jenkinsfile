pipeline {
    agent any
    
    environment {
        DOCKER_CREDENTIALS = credentials('docker-credentials')
    }
    
    tools {
        maven 'maven3'
        jdk 'jdk17'
    }
    
    stages {
        stage('Git Checkout') {
            steps {
                git credentialsId: 'github-credentials',
                    url: 'https://github.com/your-username/your-repo.git',
                    branch: 'main'
            }
        }
        
        stage('Compile') {
            steps {
                sh 'mvn clean compile'
            }
        }
        
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh 'mvn sonar:sonar'
                }
            }
        }
        
        stage('Build') {
            steps {
                sh 'mvn package -DskipTests'
            }
        }
        
        stage('Docker Build & Push') {
            steps {
                sh "docker build -t your-dockerhub-username/your-image-name:${BUILD_NUMBER} ."
                sh "echo \$DOCKER_CREDENTIALS_PSW | docker login -u \$DOCKER_CREDENTIALS_USR --password-stdin"
                sh "docker push your-dockerhub-username/your-image-name:${BUILD_NUMBER}"
            }
        }
        
        stage('Deploy') {
            steps {
                // Add your deployment steps here
                echo 'Deploying...'
            }
        }
    }
    
    post {
        always {
            cleanWs()
        }
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
