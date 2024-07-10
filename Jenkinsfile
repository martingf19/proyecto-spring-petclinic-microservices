pipeline {
    agent any

    environment {
        DOCKER_REGISTRY = 'mydockerhubaccount'
        IMAGE_NAME = 'spring-petclinic'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/spring-petclinic/spring-petclinic-microservices.git'
            }
        }

        

        stage('Build Docker Images') {
            steps {
                script {
                    docker.build("${env.DOCKER_REGISTRY}/${env.IMAGE_NAME}/config-server", "./spring-petclinic-config-server")
                    docker.build("${env.DOCKER_REGISTRY}/${env.IMAGE_NAME}/discovery-server", "./spring-petclinic-discovery-server")
                    docker.build("${env.DOCKER_REGISTRY}/${env.IMAGE_NAME}/customers-service", "./spring-petclinic-customers-service")
                    docker.build("${env.DOCKER_REGISTRY}/${env.IMAGE_NAME}/visits-service", "./spring-petclinic-visits-service")
                    docker.build("${env.DOCKER_REGISTRY}/${env.IMAGE_NAME}/vets-service", "./spring-petclinic-vets-service")
                    docker.build("${env.DOCKER_REGISTRY}/${env.IMAGE_NAME}/api-gateway", "./spring-petclinic-api-gateway")
                    docker.build("${env.DOCKER_REGISTRY}/${env.IMAGE_NAME}/admin-server", "./spring-petclinic-admin-server")
                }
            }
        }

        stage('Push Docker Images') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials') {
                        sh "docker push ${env.DOCKER_REGISTRY}/${env.IMAGE_NAME}/config-server"
                        sh "docker push ${env.DOCKER_REGISTRY}/${env.IMAGE_NAME}/discovery-server"
                        sh "docker push ${env.DOCKER_REGISTRY}/${env.IMAGE_NAME}/customers-service"
                        sh "docker push ${env.DOCKER_REGISTRY}/${env.IMAGE_NAME}/visits-service"
                        sh "docker push ${env.DOCKER_REGISTRY}/${env.IMAGE_NAME}/vets-service"
                        sh "docker push ${env.DOCKER_REGISTRY}/${env.IMAGE_NAME}/api-gateway"
                        sh "docker push ${env.DOCKER_REGISTRY}/${env.IMAGE_NAME}/admin-server"
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    sh 'docker-compose up -d'
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

