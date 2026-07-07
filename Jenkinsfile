pipeline {
    agent none
    stages {

        stage('Dev') {
            when { branch 'dev' }
            agent { label 'mirage-agent' }
            steps {

                echo "Running DEV stage on branch: ${env.BRANCH_NAME}"
          }
        }

        stage('Stage') {
            when { branch 'stage' }
            agent { label 'bumblebee-agent' }
            steps {

                echo "Running STAGE stage on branch: ${env.BRANCH_NAME}"

            }
        }

        stage('Prod') {
            when { branch 'prod' }
            agent { label 'pragyan-agent' }
            steps {

                echo "Running PROD stage on branch: ${env.BRANCH_NAME}"
            }
        }
    }

}



// pipeline {
//     agent {
//         label 'oem-agent'
//     }

//     environment {
//         REGISTRY = credentials('docker-registry-url')
//         REGISTRY_CREDENTIALS = credentials('dockerhub-creds')
//         IMAGE_NAME = 'wiseai-chat-widget-client'
//         COMMIT_SHA = sh(returnStdout: true, script: 'git rev-parse --short HEAD').trim()
//         COMMIT_HASH = sh(returnStdout: true, script: 'git rev-parse HEAD').trim()
//     }

//     stages {
//         stage('Checkout') {
//             steps {
//                 checkout scm
//                 sh 'git pull origin dev'
//             }
//         }

//         stage('Build Docker Image') {
//             steps {
//                 sh '''
//                     docker build -t ${IMAGE_NAME}:latest .
//                     docker tag ${IMAGE_NAME}:latest ${REGISTRY}/${IMAGE_NAME}:${COMMIT_SHA}
//                     docker tag ${IMAGE_NAME}:latest ${REGISTRY}/${IMAGE_NAME}:${COMMIT_HASH}
//                     docker tag ${IMAGE_NAME}:latest ${REGISTRY}/${IMAGE_NAME}:latest
//                 '''
//             }
//         }

//         stage('Push to Registry') {
//             steps {
//                 sh '''
//                     echo "${REGISTRY_CREDENTIALS_PSW}" | docker login -u "${REGISTRY_CREDENTIALS_USR}" --password-stdin ${REGISTRY}
//                     docker push ${REGISTRY}/${IMAGE_NAME}:${COMMIT_SHA}
//                     docker push ${REGISTRY}/${IMAGE_NAME}:${COMMIT_HASH}
//                     docker push ${REGISTRY}/${IMAGE_NAME}:latest
//                 '''
//             }
//         }

//         stage('Pull and Deploy') {
//             steps {
//                 sh '''
//                     docker pull ${REGISTRY}/${IMAGE_NAME}:latest
//                     docker compose down || true
//                     docker rm -f wiseai-chat-widget-client || true
//                     docker compose up -d
//                 '''
//             }
//         }
//     }

//     post {
//         always {
//             sh 'docker logout ${REGISTRY} || true'
//         }
//         success {
//             sh 'docker compose ps'
//         }
//     }
// }
