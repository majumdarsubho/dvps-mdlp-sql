pipeline {
    
    agent any
    environment {
        SQLPACKAGEPATH  = "/var/lib/jenkins/sqlpackage/sqlpackage"
        BUILDPATH       = "${WORKSPACE}/Builds/${env.JOB_NAME}-${env.BUILD_NUMBER}"
        SCRIPTPATH      = "./Scripts"
        DIR             = "${WORKSPACE}/Framework.dacpac"
        TARGETDATABASENAME = "hertz"
        host = ""
        username = ""
        password = ""
    }

    stages {
        stage('Read Secrets') {
            steps {
              script {
                env.host = sh (script: "aws secretsmanager get-secret-value --region us-east-1 --secret-id sandbox/IBMHertz/jenkins-app | jq -r .SecretString | jq -r .host", returnStdout: true)
                env.username = sh (script: "aws secretsmanager get-secret-value --region us-east-1 --secret-id sandbox/IBMHertz/jenkins-app | jq -r .SecretString | jq -r .username", returnStdout: true)
                env.password = sh (script: "aws secretsmanager get-secret-value --region us-east-1 --secret-id sandbox/IBMHertz/jenkins-app | jq -r .SecretString | jq -r .password", returnStdout: true)
              }
            }
        }
        stage('Build schema') {
            steps {
                slackSend color: '#BADA55', message: 'Schema Build Pipeline Started'
                sh'''#!/bin/bash 
                
                echo $DIR
                
                ${SQLPACKAGEPATH} /action:Publish /SourceFile:$DIR /TargetDatabaseName:${TARGETDATABASENAME} /tsn:${env.host} /tu:${env.username} /tp:${env.password}
                
                '''
                slackSend color: '#BADA55', message: 'Schema Build Successfully'
             
            }
        }
    }
}
