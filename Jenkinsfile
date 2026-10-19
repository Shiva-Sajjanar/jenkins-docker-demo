pipeline {
    agent any

    stages {
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t jenkins-docker-demo:v1 .'
            }
        }

        stage('Stop Old Container') {
            steps {
                sh 'docker rm -f jenkins-docker-website || true'
            }
        }

        stage('Deploy New Container') {
            steps {
                sh 'docker run -d --name jenkins-docker-website -p 8085:80 jenkins-docker-demo:v1'
            }
        }

        stage('Verify Website') {
            steps {
                sh 'sleep 3'
                sh 'curl --fail http://localhost:8085'
            }
        }
    }
}
