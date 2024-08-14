pipeline {
    agent any

    environment {
        DOCKER_IMAGE_NAME = 'eaazzyy/maven_app'
        DOCKER_TAG = '1.0'
        DOCKER_CREDENTIALS_ID = 'docker-login' // Use the correct credentials ID
    }

    stages {
        stage('Build Maven Artifact') {
            steps {
                sh 'echo Building Maven application...'
                sh 'mvn clean install'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image
                    sh "docker build -t ${DOCKER_IMAGE_NAME}:${DOCKER_TAG} ."
                }
            }
        }
        
        stage('Push Docker Image') {
            steps {
                script {
                    // Use withCredentials to securely provide Docker registry credentials
                    withCredentials([usernamePassword(credentialsId: DOCKER_CREDENTIALS_ID, usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        // Log in to Docker registry
                        sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'

                        // Push Docker image to registry
                        sh "docker push ${DOCKER_IMAGE_NAME}:${DOCKER_TAG}"
                    }
                }
            }
        }
    }
    
    post {
        always {
            // Clean up workspace after build
            cleanWs()
        }
    }
}
