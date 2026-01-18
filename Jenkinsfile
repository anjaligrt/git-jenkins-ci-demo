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
              echo "Starting deployment..."

              ssh ubuntu@172.31.21.5 "
                 set -e

                # Backup existing WAR
                 if [ -f /opt/tomcat/webapps/myapp.war ]; then
                    mv /opt/tomcat/webapps/myapp.war /opt/tomcat/webapps/myapp_backup.war
                 fi
              "

              # Copy new WAR
              scp target/myapp.war ubuntu@172.31.21.5:/opt/tomcat/webapps/myapp.war

              # Restart Tomcat
              ssh ubuntu@172.31.21.5 "
                 /opt/tomcat/bin/shutdown.sh || true
                 sleep 5
                 /opt/tomcat/bin/startup.sh
              "
              '''
           }
        }     

    }


    post {
    failure {
        echo "Deployment failed. Rolling back..."

        sh '''
        ssh ubuntu@172.31.21.5 "
            if [ -f /opt/tomcat/webapps/myapp_backup.war ]; then
                mv /opt/tomcat/webapps/myapp_backup.war /opt/tomcat/webapps/myapp.war
                /opt/tomcat/bin/shutdown.sh || true
                sleep 5
                /opt/tomcat/bin/startup.sh
            fi
        "
        '''
     }
    }
}
