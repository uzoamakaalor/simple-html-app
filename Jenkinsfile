pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = 'simple-html-app'
        CONTAINER_NAME = 'html-pipeline-container'
        APP_PORT = '8081'
    }
    
    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out code from GitHub...'
                checkout scm
            }
        }
        
        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                sh "docker build -t ${DOCKER_IMAGE}:${BUILD_NUMBER} ."
                sh "docker tag ${DOCKER_IMAGE}:${BUILD_NUMBER} ${DOCKER_IMAGE}:latest"
            }
        }
        
        stage('Stop Old Container') {
            steps {
                echo 'Stopping old container if exists...'
                sh "docker stop ${CONTAINER_NAME} || true"
                sh "docker rm ${CONTAINER_NAME} || true"
            }
        }
        
        stage('Deploy Application') {
            steps {
                echo 'Deploying application...'
                sh """
                    docker run -d \
                    --name ${CONTAINER_NAME} \
                    -p ${APP_PORT}:80 \
                    ${DOCKER_IMAGE}:latest
                """
            }
        }
        
        stage('Verify Deployment') {
            steps {
                echo 'Verifying deployment...'
                sh 'sleep 3'
                sh "curl -f http://localhost:${APP_PORT} || exit 1"
            }
        }
    }
    
    post {
        success {
            echo '✅ Pipeline completed successfully!'
            echo "Application running at: http://13.51.69.106:${APP_PORT}"
        }
        failure {
            echo '❌ Pipeline failed!'
        }
    }
}
