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
                sudo apt-get update && sudo apt-get install -y python3-venv  # Ensure python3-venv is installed
                if [ -f requirements.txt ]; then
                    python3 -m venv venv
                    . venv/bin/activate  # Use . instead of source for compatibility
                    pip install --break-system-packages -r requirements.txt  # Bypass managed env restrictions
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
                if [ -d tests ]; then
                    . venv/bin/activate
                    pytest tests/
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
                docker --version  # Check if Docker is installed
                docker build -t $DOCKER_IMAGE .
                docker tag $DOCKER_IMAGE $REGISTRY/$DOCKER_IMAGE:latest
                '''
                script {
                    withDockerRegistry([credentialsId: 'docker-hub-credentials', toolName: 'docker']) {
                        sh 'docker push $REGISTRY/$DOCKER_IMAGE:latest'
                    }
                }
            }
        }

        stage('Monitor') {
            steps {
                echo '📊 Monitoring application health...'
                sh '''
                if curl -s http://localhost:5000/health; then
                    echo "✅ Application is healthy."
                else
                    echo "❌ Health check failed!"
                fi
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
