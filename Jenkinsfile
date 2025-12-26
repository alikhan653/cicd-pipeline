pipeline {
    agent any

    environment {
        DOCKER_REGISTRY = 'https://registry.hub.docker.com'
        DOCKER_IMAGE = 'mybuildimage'
        DOCKER_CREDENTIALS_ID = 'docker_hub_creds_id' 
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'chmod +x scripts/build.sh'
                sh 'scripts/build.sh'
            }
        }

        stage('Test') {
            steps {
                sh 'chmod +x scripts/test.sh'
                sh 'scripts/test.sh'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t mybuildimage .'
            }
        }

        stage('Docker Push') {
            steps {
                script {
                    // Docker login
                    docker.withRegistry(DOCKER_REGISTRY, DOCKER_CREDENTIALS_ID) {
                        // Push the image with both build number and latest tag
                        def app = docker.image("${DOCKER_IMAGE}:${env.BUILD_NUMBER}")
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        }
    }
}
