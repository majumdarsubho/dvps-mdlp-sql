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
                    host=`aws secretsmanager get-secret-value --region us-east-1 --secret-id sandbox/IBMHertz/jenkins-app | jq -r .SecretString | jq -r .host`
                    
                    username=`aws secretsmanager get-secret-value --region us-east-1 --secret-id sandbox/IBMHertz/jenkins-app | jq -r .SecretString | jq -r .username`

                    password=`aws secretsmanager get-secret-value --region us-east-1 --secret-id sandbox/IBMHertz/jenkins-app | jq -r .SecretString | jq -r .password`
                    
                    echo "Host is: ${host}"
                    echo "Username: ${username}"
                    
                    ${SQLPACKAGEPATH} /action:Publish /SourceFile:$DIR /TargetDatabaseName:$TARGETDATABASENAME /tsn:$host /tu:$username /tp:$password
                    
                '''
              
            }
        }
    }
}
