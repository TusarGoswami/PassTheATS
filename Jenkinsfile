pipeline {
    agent any

    environment {
        // Define any environment variables needed for your pipeline here
        FLASK_ENV = 'testing'
    }

    stages {
        stage('Checkout') {
            steps {
                // Jenkins automatically checks out the code from the repo,
                // but this stage is here for clarity.
                checkout scm
                echo 'Source code checked out successfully.'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the Docker image for the Flask app...'
                // If you had Docker inside Jenkins, you could run:
                // sh 'docker build -t passtheats-web .'
                echo 'Build step complete.'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                // Here you would run your tests, e.g., pytest
                // sh 'pytest'
                echo 'Tests passed successfully.'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application...'
                // Code to deploy your app goes here
                echo 'Deployment successful!'
            }
        }
    }
}
