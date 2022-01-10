def version

pipeline {
    agent any
    options {
        buildDiscarder(logRotator(numToKeepStr: '30'))
    }
    environment {
        TARGET_IMG_REPOSITORY = 'harbor.prod.corp.acxiom.net/rancher'
        TARGET_REPOSITORY = 'harbor.prod.corp.acxiom.net/chartrepo/rancher'
        TARGET_REGISTRY = 'harbor.prod.corp.acxiom.net'
        USER = 'xksops'
    }
    stages {
        stage('printenv') {
            steps {
                script {
                    sh 'printenv'
                }
            }
        }
        stage ('Extract Rancher Version from Branch') {
            steps {
                sh """VERSION=\$(echo "${env.BRANCH_NAME}" | sed -e "s/^release\\///" -e "s/^v//")"""
                script {
                    version = "${VERSION}"
                }
                
                echo " --- Rancher Version: ${version} --- "
                echo " SUCCESS  "
            }
        }
    }
    post {
        success {
            sh "ls -alrt"
        }
        failure {
            // slackSend(channel: 'jenkins-spamalot', message: "app chart delivery failed! (<${env.BUILD_URL}|Open>)", color: 'danger', notifyCommitters: true, tokenCredentialId: 'slack')
            sh "echo failed"
        }
        cleanup {
            cleanWs(cleanWhenFailure: true)
        }
    }
}
