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

#        stage('Archive Artifact') {
#           steps {
#             archiveArtifacts artifacts: 'target/*.war', fingerprint: true
#           }
#        }
#
#        stage('Deploy to Tomcat') {
#           steps {
#              sh '''
#              echo "Starting deployment..."
#
#              ssh ubuntu@172.31.21.5 "
#                 set -e
#
#                # Backup existing WAR
#                 if [ -f /opt/tomcat/webapps/myapp.war ]; then
#                    mv /opt/tomcat/webapps/myapp.war /opt/tomcat/webapps/myapp_backup.war
#                 fi
#              "
#
#              # Copy new WAR
#              scp target/myapp.war ubuntu@172.31.21.5:/opt/tomcat/webapps/myapp.war
#
#              # Restart Tomcat
#              ssh ubuntu@172.31.21.5 "
#                 /opt/tomcat/bin/shutdown.sh || true
#                 sleep 5
#                 /opt/tomcat/bin/startup.sh
#              "
#              '''
#           }
#        }     

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

#    post {
#    failure {
#        echo "Deployment failed. Rolling back..."
#
#        sh '''
#        ssh ubuntu@172.31.21.5 "
#            if [ -f /opt/tomcat/webapps/myapp_backup.war ]; then
#                mv /opt/tomcat/webapps/myapp_backup.war /opt/tomcat/webapps/myapp.war
#                /opt/tomcat/bin/shutdown.sh || true
#                sleep 5
#                /opt/tomcat/bin/startup.sh
#            fi
#        "
#        '''
#     }
#    }
#}
