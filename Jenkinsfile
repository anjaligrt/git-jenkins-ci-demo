pipeline {
    agent any

    tools {
        maven 'Maven'
        jdk 'JDK17'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/anjaligrt/git-jenkins-ci-demo.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Archive Artifact') {
           steps {
             archiveArtifacts artifacts: 'target/*.war', fingerprint: true
           }
        }


        stage('Deploy to Tomcat') {
           steps {
               sh '''
               if [ -f /opt/tomcat/webapps/myapp.war ]; then
                   mv /opt/tomcat/webapps/myapp.war /opt/tomcat/webapps/myapp_backup.war
               fi
               cp target/myapp.war /opt/tomcat/webapps/
               '''
           }
        }

    }


     post {
        failure {
          echo 'Deployment failed. Rolling back...'
          sh '''
          if [ -f /opt/tomcat/webapps/myapp_backup.war ]; then
             mv /opt/tomcat/webapps/myapp_backup.war /opt/tomcat/webapps/myapp.war
          fi
          '''
        }
     }

}
