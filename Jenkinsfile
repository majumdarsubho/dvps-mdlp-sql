pipeline {
    
    agent any
    environment {
        CURRENTRELEASE  = "main"
        GITHUBCREDID    = "jenkins_pat"
        GITREPOREMOTE   = "https://github.com/majumdarsubho/dvps-mdlp-sql"
        SQLPACKAGEPATH  = "/var/lib/jenkins/sqlpackage/sqlpackage"
        BUILDPATH       = "${WORKSPACE}/Builds/${env.JOB_NAME}-${env.BUILD_NUMBER}"
        SCRIPTPATH      = "./Scripts"
        DIR             = "${WORKSPACE}/Framework.dacpac"
        HOST            = "demo-db.cof6rbxdsl87.us-east-1.rds.amazonaws.com"
        USERNAME        = "admin"
        TARGETDATABASENAME = "hertzdb"
    }

    stages {
        
        stage('Checkout') {
            steps{
              echo "Pulling ${CURRENTRELEASE} Branch from Github"
              git branch: CURRENTRELEASE, credentialsId: GITHUBCREDID, url: GITREPOREMOTE
            }
        }
        
        stage('Read Secrets and Deploy DACPAC') {
            steps {
                sh'''#!/bin/bash

                    password=`aws secretsmanager get-secret-value --region us-east-1 --secret-id sandbox/IBMHertz/jenkins-app | jq -r .SecretString | jq -r .password`
                    
                    echo "Passsword retrieved"
                    
                    ${SQLPACKAGEPATH} /action:Publish /SourceFile:$DIR /TargetDatabaseName:$TARGETDATABASENAME /tsn:$HOST /tu:$USERNAME /tp:$password
                    
                '''
              
            }
        }
    }
}
