
pipeline {
    agent any
    environment {
    
   
    TESTRESULTPATH  ="./teste_results"
    LIBRARYPATH     = "./Libraries"
    OUTFILEPATH     = "./Validation/Output"
    NOTEBOOKPATH    = "./Notebooks"
    WORKSPACEPATH   = "/Shared"
    DBFSPATH        = "dbfs:/FileStore/"
    BUILDPATH       = "${WORKSPACE}/Builds/${env.JOB_NAME}-${env.BUILD_NUMBER}"
    SCRIPTPATH      = "./Scripts"
    DIR             = "${WORKSPACE}/Framework.dacpac"
  }

    stages {
        stage('Read Secrets') {
            steps {
              script {
                username = sh (script: "aws secretsmanager get-secret-value --region us-east-1 --secret-id sandbox/IBMHertz/jenkins-app | jq -r .SecretString | jq -r .username", returnStdout: true)
                password = sh (script: "aws secretsmanager get-secret-value --region us-east-1 --secret-id sandbox/IBMHertz/jenkins-app | jq -r .SecretString | jq -r .password", returnStdout: true)
              }
            }
        }
        stage('Build schema') {
            steps {
                slackSend color: '#BADA55', message: 'Schema Build Pipeline Started'
                sh'''#!/bin/bash 
                
                echo $DIR
                
                
                '''
                slackSend color: '#BADA55', message: 'Schema Build Successfully'
             
            }
        }
    }
}
