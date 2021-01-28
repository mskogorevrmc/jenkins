#!groovy

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
                sh ''' echo 1sssss'''
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
            script {
                sh """
                    echo ${GIT_COMMIT}
                """
                env.GIT_COMMIT_MSG = sh (script: 'git log -1 --pretty=%B ${GIT_COMMIT}', returnStdout: true).trim()
                env.GIT_AUTHOR = sh (script: 'git log -1 --pretty=%ae ${GIT_COMMIT} | awk -F "@" \'{print $1}\'', returnStdout: true).trim()
                slackSend(
                    color: color_slack_msg(),
                    message: "*${currentBuild.currentResult}:* Job `${env.JOB_NAME}` build `${env.BUILD_DISPLAY_NAME}` by <@${env.GIT_AUTHOR}>\n Build commit: ${GIT_COMMIT}\n Last commit message: '${env.GIT_COMMIT_MSG}'\n More info at: ${env.BUILD_URL}\n Time: ${currentBuild.durationString.minus(' and counting')}",
                    channel: 'rmc_jenkins_ci',
                    tokenCredentialId: 'RMCSlackToken'
                )
            }
        }
    }
}


def color_slack_msg() {
    def COLOR_MAP = [
        'SUCCESS': 'good', 
        'FAILURE': 'danger',
    ]
    return COLOR_MAP[currentBuild.currentResult]
}


