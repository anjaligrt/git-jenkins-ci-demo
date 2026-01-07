pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out code'
            }
        }

        stage('Build with Maven') {
            steps {
                echo 'mvn clean package'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
		sh '''
		cp target/myapp.war /opt/tomcat/webapps/
                '''
            }
        }
    }
}

