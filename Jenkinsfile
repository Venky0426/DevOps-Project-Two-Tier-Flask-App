
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

        stage('Verify Deployment') {
            steps {
                sh '''
                    echo "Checking Flask application health..."

                    for i in $(seq 1 12); do
                        if curl --fail --silent http://localhost:5000/health; then
                            echo ""
                            echo "Application health check passed!"
                            exit 0
                        fi

                        echo "Attempt $i failed. Retrying in 5 seconds..."
                        sleep 5
                    done

                    echo "ERROR: Application health check failed!"
                    exit 1
                '''
            }
        }
    }
}
