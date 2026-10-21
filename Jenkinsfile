
pipeline {
    agent any

    stages {
        stage('Clone repo') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Venky0426/DevOps-Project-Two-Tier-Flask-App.git'
            }
        }

        stage('Build image') {
            steps {
                sh 'docker compose -p devops-project-two-tier-flask-app build'
            }
        }

        stage('Deploy with Docker Compose') {
            steps {
                sh 'docker compose -p devops-project-two-tier-flask-app up -d'
            }
        }
    }
}
