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
        }
        post {
            always {
                echo 'success'
            }
            failure {
                mailto: 'priyank.patel@magnatesage.com', subject: 'The Pipeline failed :('
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
