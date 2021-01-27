#!groovy

def COLOR_MAP = [
    'SUCCESS': 'good', 
    'FAILURE': 'danger',
]

def getBuildUser() {
    return currentBuild.rawBuild.getCause(Cause.UserIdCause).getUserId()
}


@Library('jenkins-utils-lib') _
pipeline {
    options {
        disableConcurrentBuilds()
    }
    agent {
        node { label 'rmc-build' }
     }
    stages {
        stage('Preparing') {
            steps {
                echo "Clone servicemanager repo"
            }
        }
        stage('Running tests') {
            steps {
                echo "Running tests"
            }
        }
        stage('Building') {
            steps {
                echo "Building packages"
            }
        }
        stage('Deploying') {
            steps {
                echo 'Deploying packages to Debian repository'
            }
        }
        stage('Installation') {
            steps {
                echo 'Installing'
            }
        }
        stage('Running smoke tests') {
            steps {
                echo 'Run tests'
            }
        }
        stage('Notification') {
            steps {
                echo 'Notification...'
            }
        }
    }
    post {
        always {
            cleanWs()
            script {
                BUILD_USER = getBuildUser()
                slackSend(
                    color: COLOR_MAP[currentBuild.currentResult],
                    message: "*${currentBuild.currentResult}:* Job `${env.JOB_NAME}` build `${env.BUILD_DISPLAY_NAME}` by <@${BUILD_USER}>\n Build commit: ${GIT_COMMIT}\n More info at: ${env.BUILD_URL}\n Time: ${currentBuild.durationString.minus(' and counting')}\n ${GIT_AUTHOR_NAME}",
                    channel: 'rmc_jenkins_ci',
                    tokenCredentialId: 'RMCSlackToken'
                )
            }
        }
    }
}

def notifySlack(String buildStatus = 'STARTED') {
    // Build status of null means success.
    buildStatus = buildStatus ?: 'SUCCESS'

    def color

    if (buildStatus == 'STARTED') {
        color = '#D4DADF'
    } else if (buildStatus == 'SUCCESS') {
        color = '#BDFFC3'
    } else if (buildStatus == 'UNSTABLE') {
        color = '#FFFE89'
    } else {
        color = '#FF9FA1'
    }

    def msg = "${buildStatus}: `${env.JOB_NAME}` #${env.BUILD_NUMBER}:\n${env.BUILD_URL}"

    slackSend(color: color, message: msg, channel: 'rmc_jenkins_ci', tokenCredentialId: 'RMCSlackToken')
}