pipeline {
    agent any
    
    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }
        
        stage('Test') {
            steps {
                echo 'Running syntax check on Python source code...'
                sh 'python3 -m py_compile app/main.py'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                sh 'docker build -t python-flask-app:latest .'
            }
        }
        
        stage('Deploy') {
            steps {
                echo 'Deploying application locally via Docker...'
                sh '''
                    docker stop flask-container || true
                    docker rm flask-container || true
                    docker run -d -p 5000:5000 --name flask-container python-flask-app:latest
                '''
            }
        }
    }
    
    post {
        success {
            echo 'Pipeline executed successfully with professional structure!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}