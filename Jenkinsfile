pipeline {
    agent any

    environment {
        DOCKER_REGISTRY = 'pranesh1998'
        DOCKER_IMAGE = 'NodeJS-app'
        REGISTRY_CREDENTIALS = 'docker-credentials-id' // Jenkins credentials ID for Docker registry
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/pranesh-1998/nodejs-openshift-demo.git'
            }
        }

        stage('Build') {
            steps {
                script {
                    docker.build("${DOCKER_REGISTRY}/${DOCKER_IMAGE}:${env.BUILD_NUMBER}")
                }
            }
        }

        stage('Push') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', "${REGISTRY_CREDENTIALS}") {
                        docker.image("${DOCKER_REGISTRY}/${DOCKER_IMAGE}:${env.BUILD_NUMBER}").push()
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    sh """
                    docker run -d -p 3000:3000 ${DOCKER_REGISTRY}/${DOCKER_IMAGE}:${env.BUILD_NUMBER}
                    """
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
