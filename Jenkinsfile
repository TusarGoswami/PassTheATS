pipeline {
    agent any

    environment {
        // Tag docker images with the jenkins build number
        IMAGE_NAME = "passtheats-web"
        IMAGE_TAG = "build-${BUILD_NUMBER}"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
                echo 'Source code checked out successfully.'
            }
        }

        stage('Docker Build') {
            steps {
                echo "Building the Docker image: ${IMAGE_NAME}:${IMAGE_TAG}..."
                // Build the image using the Dockerfile in the project
                sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
                sh "docker tag ${IMAGE_NAME}:${IMAGE_TAG} ${IMAGE_NAME}:latest"
            }
        }

        stage('Security / Sanity Test') {
            steps {
                echo 'Running a quick test inside the container...'
                // We run the newly built container and check if python/flask starts without crashing
                sh "docker run --rm ${IMAGE_NAME}:latest python -c \"import flask; print('Flask is installed and container works!')\""
            }
        }

        stage('Push to Docker Hub') {
            steps {
                echo 'Preparing to push to Docker Hub...'
                // We use 'docker-hub-credentials' which you will create in Jenkins UI
                withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                    
                    // 1. Login to Docker Hub safely using the credentials
                    sh 'echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin'
                    
                    // 2. Tag the image with your Docker Hub username
                    sh "docker tag ${IMAGE_NAME}:${IMAGE_TAG} ${DOCKER_USERNAME}/${IMAGE_NAME}:${IMAGE_TAG}"
                    sh "docker tag ${IMAGE_NAME}:${IMAGE_TAG} ${DOCKER_USERNAME}/${IMAGE_NAME}:latest"
                    
                    // 3. Push both the specific build tag and the 'latest' tag
                    echo 'Uploading image to Docker Hub...'
                    sh "docker push ${DOCKER_USERNAME}/${IMAGE_NAME}:${IMAGE_TAG}"
                    sh "docker push ${DOCKER_USERNAME}/${IMAGE_NAME}:latest"
                    
                    echo 'Successfully pushed to Docker Hub! 🎉'
                }
            }
        }
    }
}
