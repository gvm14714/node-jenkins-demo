pipeline {
    agent any
    environment {
        DOCKER_IMAGE = 'weezy'
        DOCKER_REGISTRY = 'gym14714'
        KUBECONFIG_CREDENTIAL_ID = 'my-kubeconfig-file'
        DOCKER_COMMAND = '/usr/local/bin/docker'
        KUBECTL_COMMAND = '/usr/local/bin/kubectl'
        NPM_COMMAND= '/usr/local/bin/npm'
    }
    stages {
        stage('Tests') {
            steps {
                echo 'Building..'
                sh "${NPM_COMMAND} npm install"
                echo 'Testing..'
                sh "${NPM_COMMAND} npm test"
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    sh "${DOCKER_COMMAND} build -t ${DOCKER_REGISTRY}/${DOCKER_IMAGE} ."
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'Dockerhub-credentials', usernameVariable: 'DOCKERHUB_USERNAME', passwordVariable: 'DOCKERHUB_PASSWORD')]) {
                        sh "${DOCKER_COMMAND} login -u ${DOCKERHUB_USERNAME} -p ${DOCKERHUB_PASSWORD}"
                        sh "${DOCKER_COMMAND} push ${DOCKER_REGISTRY}/${DOCKER_IMAGE}"
                    }
                }
            }
        }
    }
}






