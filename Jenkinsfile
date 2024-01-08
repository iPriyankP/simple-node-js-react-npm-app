pipeline {
    agent any
    environment {
        NODE_VERSION = '20.2.0'
        YARN_VERSION = '1.22.19'
        SERVER_CREDENTIALS = credentialsId('server-credentials')
    }
    stages {
        stage('development') {
            steps {
                echo 'developing the application...'
                echo "Executing Nodejs ${NODE_VERSION}"
                echo "Executing yarn ${YARN_VERSISON}"
            }
        }
        stage('Build') {
            steps {
                echo 'building the application...'
            }
        }
        stage('Test') {
            steps {
                echo 'testing the application...'
                withCredentials([
                    usernamePassword(credentialsId: 'server-credentials', usernameVariable: 'USER', passwordVariable: 'PWD')
                ]){
                    sh "some script ${USER} ${PWD}"
                }
            }
        }
        stage('Deploy') {
            steps {
                echo 'deploying the application...'
                echo "Deploying with ${SERVER_CREDENTIALS}"
            }
        }
    }
}
