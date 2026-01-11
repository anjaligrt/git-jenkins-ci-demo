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
    
 // TRIGGER BUILD

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                sh '''
                scp target/*.war ubuntu@34.233.135.189:/opt/tomcat/webapps/myapp.war
                '''
            }
        }
    }
}

