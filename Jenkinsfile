pipeline {
    
    agent any
    environment {
        SQLPACKAGEPATH  = "/var/lib/jenkins/sqlpackage/sqlpackage"
        BUILDPATH       = "${WORKSPACE}/Builds/${env.JOB_NAME}-${env.BUILD_NUMBER}"
        SCRIPTPATH      = "./Scripts"
        DIR             = "${WORKSPACE}/Framework.dacpac"
        TARGETDATABASENAME = "hertz"
        HOST = ""
        USERNAME = ""
        PASSWORD = ""
    }

    stages {
        stage('Read secrets and build schema') {
            steps {
              script {
                def host = sh (script: "aws secretsmanager get-secret-value --region us-east-1 --secret-id sandbox/IBMHertz/jenkins-app | jq -r .SecretString | jq -r .host", returnStdout: true)
                def username = sh (script: "aws secretsmanager get-secret-value --region us-east-1 --secret-id sandbox/IBMHertz/jenkins-app | jq -r .SecretString | jq -r .username", returnStdout: true)
                def password = sh (script: "aws secretsmanager get-secret-value --region us-east-1 --secret-id sandbox/IBMHertz/jenkins-app | jq -r .SecretString | jq -r .password", returnStdout: true)
                  sh (script: "sh echo ${host}")
                env.HOST = host
                env.USERNAME = username
                env.PASSWORD = password
              }
            }
        }
        stage('Build schema') {
            steps {
                
                sh'''#!/bin/bash 
                
                echo $DIR
                echo $TARGETDATABASENAME
                echo $env.HOST
                
                ${SQLPACKAGEPATH} /action:Publish /SourceFile:$DIR /TargetDatabaseName:$TARGETDATABASENAME /tsn:$env.HOST /tu:$env.USERNAME /tp:env.$PASSWORD
                
                '''
             
            }
        }
    }
}
