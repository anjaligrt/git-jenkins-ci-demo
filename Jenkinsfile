pipeline {
    agent any

    tools {
        maven 'Maven'
    }

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/anjaligrt/git-jenkins-ci-demo.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                sh '''
                scp target/*.war ubuntu@54.160.132.104:/opt/tomcat/webapps/myapp.war
                '''
            }
        }
    }
}

