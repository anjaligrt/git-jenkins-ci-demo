pipeline {
    agent any

    tools {
        maven 'Maven'
    }

    
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
