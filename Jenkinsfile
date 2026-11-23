
pipeline {
    agent any

    stages {

        stage('Git Checkout') {
            steps {
                echo 'Checking out source code'
            }
        }

        stage('Maven Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t mavenwebapp:jenkins .'
            }
        }

        stage('Docker Deploy') {
            steps {
                sh '''
                docker stop jenkins-maven || true
                docker rm jenkins-maven || true

                docker run -d \
                  --name jenkins-maven \
                  -p 8084:8080 \
                  mavenwebapp:jenkins
                '''
            }
        }
    }

    post {
        success {
            echo 'Application deployed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check Console Output.'
        }
    }
}

