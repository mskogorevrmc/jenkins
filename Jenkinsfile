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
                env.GIT_COMMIT_MSG = sh (script: 'git log -1 --pretty=%B ${GIT_COMMIT} | head -n1', returnStdout: true).stripIndent().trim()
                env.GIT_AUTHOR = sh (script: 'git log -1 --pretty=%ae ${GIT_COMMIT} | awk -F "@" \'{print $1}\'', returnStdout: true).trim()
                slackSend(
                    color: color_slack_msg(),
                    message: """
                        *${currentBuild.currentResult}:* Job `${env.JOB_NAME}` build `${env.BUILD_DISPLAY_NAME}` by <@${env.GIT_AUTHOR}>
                        Build commit: ${GIT_COMMIT}
                        Last commit message: '${env.GIT_COMMIT_MSG}'
                        More info at: ${env.BUILD_URL}
                        Time: ${currentBuild.durationString.minus(' and counting')}
                        """.stripIndent().trim(),
                    channel: 'rmc_jenkins_ci',
                    tokenCredentialId: 'RMCSlackToken'
                )
            }
        }
    }
}

def color_slack_msg() {
    switch(currentBuild.currentResult) {
    case "SUCCESS":
        return "good"
        break
    case "FAILURE":
        return "danger"
        break
    case "UNSTABLE":
        return "danger"
        break
    default:
        return "warning"
        break
    }
}
