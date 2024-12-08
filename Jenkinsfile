pipeline {
    agent any

    environment {
        IMAGE_NAME = 'my-nodejs-app'
        IMAGE_TAG = 'latest'
        SKIP_TESTS = true // Set to true to skip tests
    }

    stages {
        stage('Build') {
            steps {
                script {
                    echo 'Building Docker image...'
                    bat "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
                }
            }
        }
        stage('Test') {
            when {
                expression { return !env.SKIP_TESTS.toBoolean() }  // Skip tests if SKIP_TESTS is true
            }
            steps {
                script {
                    echo 'Running tests...'
                    bat "docker run --rm ${IMAGE_NAME}:${IMAGE_TAG} npm test"
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    echo 'Deploying application...'
                    bat "docker tag ${IMAGE_NAME}:${IMAGE_TAG} your-dockerhub-username/${IMAGE_NAME}:${IMAGE_TAG}"
                    bat "docker push your-dockerhub-username/${IMAGE_NAME}:${IMAGE_TAG}"
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution complete.'
        }
        failure {
            echo 'Pipeline failed. Check the logs for details.'
        }
    }
}
