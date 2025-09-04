pipeline {
    agent any

    tools {
        maven 'Maven'   // Configure Maven in Jenkins > Global Tool Configuration
    }

    environment {
        APP_NAME = "demo-app"
        DOCKER_IMAGE = "demo-app:latest"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/saiteja-feed/demo.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Package') {
            steps {
                sh 'mvn package'
            }
        }

        stage('Docker Build') {
            steps {
                sh "docker build -t ${DOCKER_IMAGE} -f dockerfile ."
            }
        }

        stage('Run Container') {
            steps {
                sh """
                   docker ps -q --filter "name=${APP_NAME}" | grep -q . && docker stop ${APP_NAME} && docker rm ${APP_NAME} || true
                   docker run -d --name ${APP_NAME} -p 8081:8080 ${DOCKER_IMAGE}
                """
            }
        }
    }

    post {
        success {
            echo "✅ Pipeline completed successfully!"
        }
        failure {
            echo "❌ Pipeline failed. Check logs."
        }
    }
}
