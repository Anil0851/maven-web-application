
pipeline {
    agent any

    stages {

        stage('Git Checkout') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/prasads1283/maven-web-application.git'
            }
        }

        stage('Maven Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t mavenwebapp:v1 .'
            }
        }

        stage('Docker Deploy') {
            steps {
                sh '''
                docker stop maven || true
                docker rm maven || true
                docker run -d --name maven -p 8081:8080 mavenwebapp:v1
                '''
            }
        }
    }

    post {
        success {
            echo 'Application deployed successfully!'
        }

        failure {
            echo 'Pipeline failed. Check Jenkins console output.'
        }
    }
}

