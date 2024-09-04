pipeline {
    agent any
    environment {
        S3_BUCKET = 'mywebapp-deploy-bucket'  // Replace with your actual bucket name
        APPLICATION_NAME = 'MyWebApp'
        DEPLOYMENT_GROUP_NAME = 'MY-DEPLOYMRNT-GROUP'
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/naikpradyumna295/Project-Oriserve.git'
            }
        }
        stage('Package') {
            steps {
                sh 'zip -r MyWebApp.zip *'
                archiveArtifacts artifacts: 'MyWebApp.zip', fingerprint: true
            }
        }
        stage('Upload to S3') {
            steps {
                withAWS(credentials: 'aws-credentials') {
                    s3Upload(bucket: "${S3_BUCKET}", file: 'MyWebApp.zip')
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    def deployApp = awsCodeDeploy applicationName: "${APPLICATION_NAME}",
                                                  deploymentGroupName: "${DEPLOYMENT_GROUP_NAME}",
                                                  s3bucket: "${S3_BUCKET}",
                                                  s3key: 'MyWebApp.zip'
                }
            }
        }
    }
}

