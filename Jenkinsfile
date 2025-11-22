pipeline {
    agent any
    
    environment {
        DOCKERHUB_REPO = 'uzoamakaalor/simple-html-app'
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
                sh "docker tag ${DOCKER_IMAGE}:${BUILD_NUMBER} ${DOCKERHUB_REPO}:${BUILD_NUMBER}"
                sh "docker tag ${DOCKER_IMAGE}:${BUILD_NUMBER} ${DOCKERHUB_REPO}:latest"
            }
        }
        
        stage('Push to Docker Hub') {
            steps {
                echo 'Pushing Docker image to Docker Hub...'
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials') {
                        sh "docker push ${DOCKERHUB_REPO}:${BUILD_NUMBER}"
                        sh "docker push ${DOCKERHUB_REPO}:latest"
                    }
                }
                echo "Image pushed: ${DOCKERHUB_REPO}:${BUILD_NUMBER}"
                echo "Image pushed: ${DOCKERHUB_REPO}:latest"
            }
        }
        
        stage('Stop Old Container') {
            steps {
                echo 'Stopping old container...'
                sh "docker stop ${CONTAINER_NAME} || true"
                sh "docker rm ${CONTAINER_NAME} || true"
            }
        }
        
        stage('Deploy from Docker Hub') {
            steps {
                echo 'Deploying application from Docker Hub...'
                sh """
                    docker run -d \
                    --name ${CONTAINER_NAME} \
                    -p ${APP_PORT}:80 \
                    ${DOCKERHUB_REPO}:latest
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
            echo "Application: http://13.51.69.106:${APP_PORT}"
            echo "Docker Hub: https://hub.docker.com/r/${DOCKERHUB_REPO}"
        }
        failure {
            echo '❌ Pipeline failed!'
        }
    }
}
