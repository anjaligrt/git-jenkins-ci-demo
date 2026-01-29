pipeline {
    agent any

    environment {
        IMAGE_NAME = "jenkins-maven-demo"
        CONTAINER_NAME = "demo-app"
    }

#    tools {
#        maven 'Maven'
#        jdk 'JDK17'
#    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/anjaligrt/git-jenkins-ci-demo.git'
            }
        }
        
        stage('Build with Maven (Dockerized)') {
            agent {
                docker {
                    image 'maven:3.9.9-eclipse-temurin-21'
                    args '-v /root/.m2:/root/.m2'
                }
            }
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
                sh 'docker build -t $IMAGE_NAME:latest .'
            }
        }

        stage('Stop Old Container (if exists)') {
            steps {
                sh '''
                docker stop $CONTAINER_NAME || true
                docker rm $CONTAINER_NAME || true
                '''
            }
        }
        stage('Run Docker Container') {
            steps {
                sh '''
                docker run -d \
                --name $CONTAINER_NAME \
                -p 9090:8080 \
                $IMAGE_NAME:latest
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
