pipeline {
    agent any

    environment {
        MAVEN_HOME = '/usr/share/maven'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/lily4499/sample-java-app.git'
            }
        }
        stage('Build') {
            steps {
                sh "${MAVEN_HOME}/bin/mvn clean install"
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('Your-SonarQube-Server-Name') {
                    // Using sonar-scanner with project settings file
                    sh 'sonar-scanner -Dproject.settings=sonar-project.properties'
                }
            }
        }
        stage('Deploy to Artifactory') {
            steps {
                sh "${MAVEN_HOME}/bin/mvn deploy"
            }
        }
    }
    post {
        always {
            echo 'Pipeline completed.'
        }
    }
}
