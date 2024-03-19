pipeline {
    agent any
    tools { 
        nodejs "Node-JS" 
    }
    environment {
        DOCKER_IMAGE = 'weezy'
        DOCKER_REGISTRY = 'gym14714'
        KUBECONFIG_CREDENTIAL_ID = 'my-kubeconfig-file'
        DOCKER_COMMAND = '/usr/local/bin/docker'
        KUBECTL_COMMAND = '/usr/local/bin/kubectl'
    }
    stages {
        stage('Tests') {
            steps {
                echo 'Building..'
                sh 'npm install'
                echo 'Testing..'
                sh 'npm test'
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
         stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Use the kubeconfig file
                    withCredentials([file(credentialsId: KUBECONFIG_CREDENTIAL_ID, variable: 'KUBECONFIG')]) {
                        // Deploy to Kubernetes using kubectl commands
                        sh "${KUBECTL_COMMAND} apply -f cash.yml"
                    }
                }
            }
        }

    }
}







