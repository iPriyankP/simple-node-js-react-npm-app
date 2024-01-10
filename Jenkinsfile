pipeline {
    agent any
    environment {
        NODE_VERSION = '20.2.0'
        YARN_VERSION = '1.22.19'
        SERVER_CREDENTIALS = credentials('server-credentials')
        BRANCH_NAME = "${GIT_BRANCH.split('/').size() > 1 ? GIT_BRANCH.split('/')[1..-1].join('/') : GIT_BRANCH}"
    }
    parameters {
        choice(name: 'VERSION', choices: ['1.1.0', '1.2.0', '1.3.0'] , description: '')
        booleanParam(name: 'executeTest', defaultValue: true, description: '')
    }
    stages {
        stage('branch check') {
            steps {
                def CURRENT_BRANCH=env.BRANCH_NAME
                if (CURRENT_BRANCH === 'master') {
                    echo "current branch ${BRANCH_NAME}"
                } else if (CURRENT_BRANCH === 'dev') {
                    echo "current branch ${BRANCH_NAME} CURRENT_BRANCH"
                } else if (CURRENT_BRANCH === 'staging') {
                    echo "current branch ${BRANCH_NAME}"
                }
                echo 'developing the application...'
                echo "Executing Nodejs ${NODE_VERSION}"
                echo "Executing yarn ${YARN_VERSION}"
                echo "Branch Name ${BRANCH_NAME}"
            }
        }
        stage('Build') {
            steps {
                echo 'building the application...'
                echo ''
                echo "GIT_COMMIT ${GIT_COMMIT}" 
                echo "GIT_BRANCH ${GIT_BRANCH}"
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
                        to: 'priyank.magnates@gmail.com, priyank.patel@magnatesage.com'
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
