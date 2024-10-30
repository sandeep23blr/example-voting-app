pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'python-docker-app'  // Docker image name
        DOCKER_FILE = 'vote/Dockerfile'      // Path to the Dockerfile
        GIT_REPO = 'https://github.com/sandeep23blr/example-voting-app.git' // Your GitHub repository
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Checkout the code from the GitHub repository
                git url: GIT_REPO, branch: 'main'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    sh "docker build -t ${DOCKER_IMAGE} -f ${DOCKER_FILE} ."
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    // Run the Docker container
                    // Adjust the port mapping if necessary
                    sh "docker run -d -p 80:80 ${DOCKER_IMAGE}"
                }
            }
        }
    }

    post {
        always {
            // Clean up Docker images and containers after the build
            sh "docker rm -f \$(docker ps -aq)" // Remove all containers
            sh "docker rmi -f ${DOCKER_IMAGE}" // Remove the Docker image
        }

        success {
            echo 'Deployment succeeded!'
        }

        failure {
            echo 'Deployment failed. Check the logs for details.'
        }
    }
}
