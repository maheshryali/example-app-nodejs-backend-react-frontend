pipeline {
    agent any
    tools {
        jdk 'jdk17'
        nodejs 'nodejsv12'
    }
    triggers { pollSCM('* * * * *') }
    stages {
        stage('clone_the_code') {
            steps {
                git branch: 'master',
                       url: 'https://github.com/maheshryali/example-app-nodejs-backend-react-frontend.git'
            }
        }
        stage('sonar_scan') {
            steps {
                withSonarQubeEnv('sonar') {
                    sh ''' $SCANNER_HOME/bin/sonarscanner -Dsonar.projectName=example \
                    -Dsonar.projectKey=example '''
                }
            }
        }
    }
}