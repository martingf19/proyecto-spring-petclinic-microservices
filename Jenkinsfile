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
/*
        stage('Build Docker Images') {
            steps {
                script {
                    def services = [
                        'config-server',
                        'discovery-server',
                        'customers-service',
                        'visits-service',
                        'vets-service',
                        'api-gateway',
                        'admin-server'
                    ]
                    for (service in services) {
                        def servicePath = "./spring-petclinic-${service}"
                        if (fileExists("${servicePath}/Dockerfile")) {
                            docker.build("${env.DOCKER_REGISTRY}/${env.IMAGE_NAME}/${service}", servicePath)
                        } else {
                            error("Dockerfile not found in ${servicePath}")
                        }
                    }
                }
            }
        }

        stage('Push Docker Images') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials') {
                        def services = [
                            'config-server',
                            'discovery-server',
                            'customers-service',
                            'visits-service',
                            'vets-service',
                            'api-gateway',
                            'admin-server'
                        ]
                        for (service in services) {
                            sh "docker push ${env.DOCKER_REGISTRY}/${env.IMAGE_NAME}/${service}"
                        }
                    }
                }
            }
        }
*/

        stage('Install Docker Compose') {
            steps {
                sh 'curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose'
                sh 'chmod +x /usr/local/bin/docker-compose'
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
