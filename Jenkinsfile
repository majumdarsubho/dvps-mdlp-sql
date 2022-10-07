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
        stage('Read secrets and build schema') {
            steps {
              script {
                host = sh (script: "aws secretsmanager get-secret-value --region us-east-1 --secret-id sandbox/IBMHertz/jenkins-app | jq -r .SecretString | jq -r .host", returnStdout: true)
                username = sh (script: "aws secretsmanager get-secret-value --region us-east-1 --secret-id sandbox/IBMHertz/jenkins-app | jq -r .SecretString | jq -r .username", returnStdout: true)
                password = sh (script: "aws secretsmanager get-secret-value --region us-east-1 --secret-id sandbox/IBMHertz/jenkins-app | jq -r .SecretString | jq -r .password", returnStdout: true)
              }
              sh'''#!/bin/bash 
                
                echo $DIR
                echo $TARGETDATABASENAME
                echo $username
                echo $password
                
                ${SQLPACKAGEPATH} /action:Publish /SourceFile:$DIR /TargetDatabaseName:$TARGETDATABASENAME /tsn:$host /tu:$username /tp:$password
                
                '''
            }
        }
        stage('Build schema') {
            steps {
                slackSend color: '#BADA55', message: 'Schema Build Pipeline Started'
                
                slackSend color: '#BADA55', message: 'Schema Build Successfully'
             
            }
        }
    }
}
