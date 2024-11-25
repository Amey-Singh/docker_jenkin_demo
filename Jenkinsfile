pipeline {
    agent any
    environment {
        GIT_REPOSITORY_URL = 'https://github.com/Amey-Singh/docker_jenkin_demo.git'
        DOCKER_IMAGE_NAME = 'AmeySingh/docker_jenkis_demo'
        IMAGE_TAG = '1.0'
    }
    stages {
        stage('Clone Repository') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'github-token', usernameVariable: 'GITHUB_USERNAME', passwordVariable: 'GITHUB_TOKEN')]) {
                        sh """
                            git clone https://${GITHUB_USERNAME}:${GITHUB_TOKEN}@${GIT_REPOSITORY_URL.replace('https://', '')} .
                        """
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    try {
                        docker.build("${DOCKER_IMAGE_NAME}:${IMAGE_TAG}")
                    } catch (Exception e) {
                        echo "Failed to build Docker image: ${e.message}"
                        error "Failed to build Docker image"
                    }
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
                    try {
                        withCredentials([usernamePassword(credentialsId: 'my-docker-hub-credentials-id', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                            sh """
                                echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin
                                docker push ${DOCKER_IMAGE_NAME}:${IMAGE_TAG}
                            """
                        }
                    } catch (Exception e) {
                        echo "Failed to push Docker image to registry: ${e.message}"
                        error "Failed to push Docker image"
                    }
                }
            }
        }
    }
}
