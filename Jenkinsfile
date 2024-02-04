pipeline {
    agent any
    tools {
        jdk 'jdk17'
        nodejs 'nodejsv12'
    }
    environment {
        SCANNER_HOME=tool 'sonar-scanner'
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
                    sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=examplenodejs \
                    -Dsonar.projectKey=examplenodejs '''
                }
            }
        }
        stage('Quality_gate') {
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-token'
                }
            }
        }
        stage('Install dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Docker_build_push') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'dockercred', toolName: 'docker') {
                        docker image build -t nodejs:${BUILD_ID} .
                        docker tag nodejs:${BUILD_ID} maheshryali/nodejs:${BUILD_ID}
                        docker image push maheshryali/nodejs:${BUILD_ID}
                    }
                }
            }
        }
    }
}