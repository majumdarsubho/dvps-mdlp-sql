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
              sh'''#!/bin/bash
              
              host = aws secretsmanager get-secret-value --region us-east-1 --secret-id sandbox/IBMHertz/jenkins-app | jq -r .SecretString | jq -r .host
              
              echo $host
              '''
            }
        }
        stage('Build schema') {
            steps {
                
                sh'''#!/bin/bash 
                
                echo $DIR
                echo $TARGETDATABASENAME
                
                ${SQLPACKAGEPATH} /action:Publish /SourceFile:$DIR /TargetDatabaseName:$TARGETDATABASENAME /tsn:demo-db.cof6rbxdsl87.us-east-1.rds.amazonaws.com /tu:$username /tp:IBMHertz-Project121
                
                '''
             
            }
        }
    }
}
