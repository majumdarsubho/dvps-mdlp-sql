pipeline {
    
    agent any
    environment {
        SQLPACKAGEPATH  = "/var/lib/jenkins/sqlpackage/sqlpackage"
        BUILDPATH       = "${WORKSPACE}/Builds/${env.JOB_NAME}-${env.BUILD_NUMBER}"
        SCRIPTPATH      = "./Scripts"
        DIR             = "${WORKSPACE}/Framework.dacpac"
        TARGETDATABASENAME = "hertzdb"
    }

    stages {
        stage('Read Secrets and Deploy DACPAC') {
            steps {
                sh'''#!/bin/bash
                    host=`aws secretsmanager get-secret-value --region us-east-1 --secret-id sandbox/IBMHertz/jenkins-app | jq -r .SecretString | jq -r .host`
                    
                    username=`aws secretsmanager get-secret-value --region us-east-1 --secret-id sandbox/IBMHertz/jenkins-app | jq -r .SecretString | jq -r .username`

                    password=`aws secretsmanager get-secret-value --region us-east-1 --secret-id sandbox/IBMHertz/jenkins-app | jq -r .SecretString | jq -r .password`
                    
                    echo "${host}"
                    echo "${username}"
                    echo "${password}"
                    
                    ${SQLPACKAGEPATH} /action:Publish /SourceFile:$DIR /TargetDatabaseName:$TARGETDATABASENAME /tsn:$host /tu:$username /tp:$password
                    
                '''
              
            }
        }
    }
    post {
		success {
		  withAWS(credentials:'AWSCredentialsForSnsPublish') {
				snsPublish(
					topicArn:'arn:aws:sns:us-east-1:872161624847:mdlp-build-status-topic', 
					subject:"Job:${env.JOB_NAME}-Build Number:${env.BUILD_NUMBER} is a ${currentBuild.currentResult}", 
					message: "Please note that for Jenkins job:${env.JOB_NAME} of build number:${currentBuild.number} - ${currentBuild.currentResult} happened!"
				)
			}
		}
		failure {
		  withAWS(credentials:'AWSCredentialsForSnsPublish') {
				snsPublish(
					topicArn:'arn:aws:sns:us-east-1:872161624847:mdlp-build-status-topic', 
					subject:"Job:${env.JOB_NAME}-Build Number:${env.BUILD_NUMBER} is a ${currentBuild.currentResult}", 
					message: "Please note that for Jenkins job:${env.JOB_NAME} of build number:${currentBuild.number} - ${currentBuild.currentResult} happened! Details here: ${BUILD_URL}."
				)
			}
		}
  }
}
