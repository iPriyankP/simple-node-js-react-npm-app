pipeline {
    agent any
    environment {
        NODE_VERSION = '20.2.0'
        YARN_VERSION = '1.22.19'
        SERVER_CREDENTIALS = credentials('server-credentials')
    }
    parameters {
        choice(name: 'VERSION', choices: ['1.1.0', '1.2.0', '1.3.0'] , description: '')
        booleanParam(name: 'executeTest', defaultValue: true, description: '')
    }
    stages {
        stage('development') {
            steps {
                echo 'developing the application...'
                echo "Executing Nodejs ${NODE_VERSION}"
                echo "Executing yarn ${YARN_VERSION}"
            }
        }
        stage('Build') {
            steps {
                echo 'building the application...'
                @echo off
                echo GIT_COMMIT %GIT_COMMIT% 
                echo GIT_BRANCH %GIT_BRANCH%
                echo GIT_LOCAL_BRANCH %GIT_LOCAL_BRANCH%
                echo GIT_PREVIOUS_COMMIT %GIT_PREVIOUS_COMMIT%
                echo GIT_PREVIOUS_SUCCESSFUL_COMMIT %GIT_PREVIOUS_SUCCESSFUL_COMMIT%
                echo GIT_URL %GIT_URL%
                echo GIT_URL_N - %GIT_URL_N%
                echo GIT_AUTHOR_NAME %GIT_AUTHOR_NAME%
                echo GIT_COMMITTER_EMAIL %GIT_COMMITTER_EMAIL%
            }
        }
        stage('Test') {
            when {
                expression {
                    params.executeTest
                }
            }
            steps {
                echo 'testing the application...'
                withCredentials([
                    usernamePassword(credentialsId: 'server-credentials', usernameVariable: 'USER', passwordVariable: 'PWD')
                ]){
                    echo "${USER} ${PWD}"
                }
            }
            post {
                always {
                    echo 'success'
                    emailext body:'Test Success Message. This is auto generated email from Jenkins from pipeline. Do not Reply.',
                        subject: 'The Pipeline successfully executed Test stage :)',
                        to: 'priyank.magnates@gmail.com, priyank.patel@magnatesage.com, nevil.gambhava@magnatesage.com'
                }
                failure {
                    emailext body:'Test Message',
                        subject: 'The Pipeline failed :(',
                        to: 'priyankpatel@magnatesage.com'
                }
            }
        }
        stage('Deploy') {
            steps {
                echo 'deploying the application...'
                echo "${SERVER_CREDENTIALS}"
                echo "deploying version ${params.VERSION}"
            }
        }
    }
}
