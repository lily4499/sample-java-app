pipeline {
    agent any

    environment {
        MAVEN_HOME = '/usr/share/maven'
        GIT_REPO = 'https://github.com/lily4499/sample-java-app.git'  // Corrected repository URL
        GIT_BRANCH = 'main'  // Replace with your branch name if different
    }

    stages {
        stage('Clean Workspace') {
            steps {
                cleanWs()  // Jenkins built-in function to clean the workspace
            }
        }

        stage('Clone Repository') {
            steps {
                git branch: "${env.GIT_BRANCH}",
                    url: "${env.GIT_REPO}",
                    credentialsId: 'github-token'  // Replace with your Jenkins GitHub credentials ID
            }
        }

        stage('Build') {
            steps {
                sh "${env.MAVEN_HOME}/bin/mvn clean install"
            }
        }

         stage('Run Sonarqube') {
            environment {
                scannerHome = tool 'sonar-scanner';
            }
            steps {
              withSonarQubeEnv(credentialsId: 'sonar-token', installationName: 'sonar-server') {
                sh "${scannerHome}/bin/sonar-scanner -Dproject.settings=sonar-project.properties"
              }
            }
        }

        // stage('SonarQube Analysis') {
        //     steps {
        //         withSonarQubeEnv('sonar-server') {  // Replace with your SonarQube server name
        //             // Using sonar-scanner with project settings file
        //             sh 'sonar-scanner -Dproject.settings=sonar-project.properties'
        //         }
        //     }
        // }

        stage('Deploy to Artifactory') {
            steps {
                withCredentials([file(credentialsId: 'jfrog-credentials', variable: 'SETTINGS_FILE')]) {
                    sh """
                        mvn deploy -s $SETTINGS_FILE
                    """
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed.'
        }
    }
}
