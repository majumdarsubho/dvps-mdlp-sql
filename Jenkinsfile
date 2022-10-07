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
                sh'''#!/bin/bash
                    echo "$(aws secretsmanager get-secret-value --region us-east-1 --secret-id sandbox/IBMHertz/jenkins-app | jq -r .SecretString | jq -r .host)" >> $HOST

                    echo "$(aws secretsmanager get-secret-value --region us-east-1 --secret-id sandbox/IBMHertz/jenkins-app | jq -r .SecretString | jq -r .username)" >> $USERNAME

                    echo "$(aws secretsmanager get-secret-value --region us-east-1 --secret-id sandbox/IBMHertz/jenkins-app | jq -r .SecretString | jq -r .password)" >> $PASSWORD
                    

                    echo $DIR
                    echo $HOST
                    echo $USERNAME
                    echo $PASSWORD
                    
                '''
              
            }
        }
        stage('Build schema') {
            steps {
                
                sh'''#!/bin/bash 
                
                
                echo $TARGETDATABASENAME
                
                '''
             
            }
        }
    }
}
