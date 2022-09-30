
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
        stage('Build schema') {
            steps {
                slackSend color: '#BADA55', message: 'Schema Build Pipeline Started'
                sh'''#!/bin/bash 
                
                echo $DIR
                
                /var/lib/jenkins/sqlpackage/sqlpackage /action:Publish /SourceFile:$DIR /TargetDatabaseName:hertz /tsn:demo-db.cof6rbxdsl87.us-east-1.rds.amazonaws.com /tu:admin /tp:"IBMHertz-Project121"
                '''
                slackSend color: '#BADA55', message: 'Schema Build Successfully'
             
            }
        }
    }
}
