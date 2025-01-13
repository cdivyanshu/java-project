pipeline {
    agent any

    environment {
        REGISTRY_URL = 'your-harbor-registry-url'
        IMAGE_NAME = 'demo-app'
        IMAGE_TAG = 'latest'
        DOCKER_CREDENTIALS = 'harbor-credentials' // Jenkins credentials ID
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://your-git-repo-url.git'
            }
        }

        stage('Build Java Application') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ${REGISTRY_URL}/${IMAGE_NAME}:${IMAGE_TAG} .'
            }
        }

        stage('Push to Harbor Registry') {
            steps {
                withDockerRegistry(credentialsId: "${DOCKER_CREDENTIALS}", url: "https://${REGISTRY_URL}") {
                    sh 'docker push ${REGISTRY_URL}/${IMAGE_NAME}:${IMAGE_TAG}'
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
