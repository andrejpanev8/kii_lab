pipeline {
    agent any  // This tells Jenkins to run the pipeline on any available agent
    environment {
        DOCKER_REGISTRY = 'https://registry.hub.docker.com'
        DOCKER_CREDENTIALS = 'my-dockerhub'
    }
    stages {
        stage('Clone repository') {
            steps {
                // Checkout the code from the repository
                checkout scm
            }
        }
        stage('Build image') {
            steps {
                script {
                    // Build the Docker image
                    app = docker.build("andrejpanev8/kii_lab")
                }
            }
        }
        stage('Push image') {
            steps {
                script {
                    // Push the Docker image to Docker Hub with the branch and build number tags
                    docker.withRegistry(DOCKER_REGISTRY, DOCKER_CREDENTIALS) {
                        app.push("${env.BRANCH_NAME}-${env.BUILD_NUMBER}")
                        app.push("${env.BRANCH_NAME}-latest")
                    }
                }
            }
        }
    }
    post {
        always {
            // Any cleanup or notifications can go here
            echo 'Pipeline finished.'
        }
    }
}
