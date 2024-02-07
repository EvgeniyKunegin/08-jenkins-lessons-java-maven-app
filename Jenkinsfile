#!/user/bin/env groovy

library identifier: 'Jenkins-Shared-library@main', retriever: modernSCM(
        [$class: 'GitSCMSource',
        remote: 'https://github.com/EvgeniyKunegin/08-jenkins-lessons_Jenkins-Shared-library.git',
        credentialsID: 'github-credential'])

def gv

pipeline {
    agent any
    tools {
        maven "maven 3.9.5"
    }
    stages {
        stage("init") {
            steps {
                script {
                    gv = load "script.groovy"
                }
            }
        }
        stage("build jar") {
            steps {
                script {
                    buildJar()
                }
            }
        }

        stage("build and push image") {
            steps {
                script {
                    buildImage 'evgenk25/demo-app:jma-3.0'
                    dockerLogin()
                    dockerPush 'evgenk25/demo-app:jma-3.0'
                }
            }
        }

        stage("deploy") {
            steps {
                script {
                    gv.deployApp()
                }
            }
        }
    }
} 
