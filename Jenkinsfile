pipeline {
    agent any

    environment {
        REGISTRY_CREDENTIALS = credentials('dockerhub-creds')
        REGISTRY = 'docker.io/robert803556'
        IMAGE_NAME = 'wiseai-chat-widget-client'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
                sh 'git pull origin dev'
                script {
                    env.COMMIT_HASH = sh(returnStdout: true, script: 'git rev-parse HEAD').trim()
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    docker build -t ${IMAGE_NAME}:latest .
                    docker tag ${IMAGE_NAME}:latest ${REGISTRY}/${IMAGE_NAME}:${COMMIT_HASH}
                    docker tag ${IMAGE_NAME}:latest ${REGISTRY}/${IMAGE_NAME}:latest
                '''
            }
        }

        stage('Push to Registry') {
            steps {
                sh '''
                    echo "${REGISTRY_CREDENTIALS_PSW}" | docker login -u "${REGISTRY_CREDENTIALS_USR}" --password-stdin ${REGISTRY}
                    docker push ${REGISTRY}/${IMAGE_NAME}:${COMMIT_HASH}
                    docker push ${REGISTRY}/${IMAGE_NAME}:latest
                '''
            }
        }

        stage('Pull and Deploy') {
            steps {
                sh '''
                    docker pull ${REGISTRY}/${IMAGE_NAME}:latest
                    docker compose down || true
                    docker rm -f wiseai-chat-widget-client || true
                    docker compose up -d
                '''
            }
        }
    }

    post {
        always {
            sh 'docker logout ${REGISTRY} || true'
        }
        success {
            sh 'docker compose ps'
        }
    }
}
