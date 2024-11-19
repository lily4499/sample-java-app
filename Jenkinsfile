pipeline {
    agent any

    environment {
        MAVEN_HOME = '/usr/share/maven'
         GIT_REPO = 'https://github.com/lily4499/samnple-java-app.git'  // Replace with your repository URL
        GIT_BRANCH = 'main'  // Replace with your branch name if different
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: "${env.GIT_BRANCH}",
                    url: "${env.GIT_REPO}",
                    credentialsId: 'github-token'  // Replace with your Jenkins GitHub credentials ID
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
