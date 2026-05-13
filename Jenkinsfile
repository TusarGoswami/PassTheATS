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

        stage('Deploy (Mock / AWS Preparation)') {
            steps {
                echo 'Image is ready for AWS deployment!'
                echo "To push to AWS ECR, we would run: docker push aws-account-id.dkr.ecr.region.amazonaws.com/${IMAGE_NAME}:${IMAGE_TAG}"
            }
        }
    }
}
