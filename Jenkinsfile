#!groovy?
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
        }
    }
}

slackSend botUser: true, 
  channel: 'rmc_jenkins_ci', 
  color: '#00ff00', 
  message: 'Testing Jekins with Slack', 
  tokenCredentialId: 'xoxb-49626827319-1663186210161-7hMrTj96WtoQLGEcd6p9RJsF'