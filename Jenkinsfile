pipeline {
    agent any

    environment {
        IMAGE_NAME = "jenkins-maven-demo"
        CONTAINER_NAME = "demo-app"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/anjaligrt/git-jenkins-ci-demo.git'
            }
        }
        
        stage('Build with Maven (Dockerized)') {
            steps {
                sh 'mvn clean package'
            }
        }


        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t jenkins-maven-demo .
                '''
            }
        }

        stage('Stop Old Container (if exists)') {
            steps {
                sh '''
                docker stop demo-app || true
                docker rm demo-app || true
                '''
            }
        }
        stage('Run Docker Container') {
            steps {
                sh '''
                docker run -d -p 8081:8080 --name demo-app jenkins-maven-demo
                '''
            }
        }
    }

    post {
        success {
            echo "Deployment successful!"
        }
        failure {
            echo "Pipeline failed. Check logs."
        }
    }
}
