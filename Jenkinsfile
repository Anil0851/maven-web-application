

pipeline {
    agent any

    environment {
        IMAGE_NAME = 'mavenwebapp'
        CONTAINER_NAME = 'maven'
        HOST_PORT = '8081'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/Anil0851/maven-web-application.git'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Verify WAR') {
            steps {
                sh 'ls -lh target/*.war'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t mavenwebapp:latest .'
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                    docker rm -f maven || true

                    docker run -d \
                      --name maven \
                      --restart unless-stopped \
                      -p 8081:8080 \
                      mavenwebapp:latest
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                sh 'docker ps'
                sh 'curl -f http://localhost:8084/'
            }
        }
    }
}

