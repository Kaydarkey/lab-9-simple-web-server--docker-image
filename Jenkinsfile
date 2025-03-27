pipeline {
    agent any

    environment {
        REPO_URL = 'https://github.com/Kaydarkey/lab-9-simple-web-server--docker-image.git'
        DOCKER_IMAGE = 'flask-app'
        REGISTRY = 'kaydar'
    }

    stages {
        stage('Source') {
            steps {
                echo '📥 Cloning the repository...'
                git url: env.REPO_URL, branch: 'main'
            }
        }

        stage('Build') {
            steps {
                echo '📦 Installing dependencies...'
                sh '''
                    set -e  # Exit on error
                    sudo apt-get update && sudo apt-get install -y python3-venv  # Ensure python3-venv is installed
                    
                    if [ -f requirements.txt ]; then
                        sudo python3 -m venv venv  # Create virtual environment
                        sudo venv/bin/python -m pip install --upgrade pip  # Upgrade pip
                        sudo venv/bin/python -m pip install -r requirements.txt  # Install dependencies
                    else
                        echo "⚠️ requirements.txt not found. Skipping dependency installation."
                    fi
                '''
            }
        }

        stage('Test') {
            steps {
                echo '🧪 Running tests...'
                sh '''
                    set -e
                    if [ -d tests ]; then
                        venv/bin/python -m pytest tests/
                    else
                        echo "⚠️ No tests directory found. Skipping tests."
                    fi
                '''
            }
        }

        stage('Deploy') {
            steps {
                echo '🚀 Building and deploying Docker image...'
                sh '''
                    set -e
                    docker build -t $DOCKER_IMAGE .
                    docker tag $DOCKER_IMAGE $REGISTRY/$DOCKER_IMAGE:latest
                '''
                script {
                    withDockerRegistry([credentialsId: 'docker-hub-credentials', toolName: 'docker']) {
                        sh "docker push ${env.REGISTRY}/${env.DOCKER_IMAGE}:latest"
                    }
                }
            }
        }

        stage('Monitor') {
            steps {
                echo '📊 Monitoring application health...'
                sh '''
                    curl -s http://localhost:5000/health && echo "✅ Application is healthy." || echo "❌ Health check failed!"
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline completed successfully!'
        }
        failure {
            echo '❌ Pipeline failed. Check the logs for errors.'
        }
    }
}
