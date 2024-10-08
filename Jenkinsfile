pipeline {
    agent any
    environment {
        S3_BUCKET = 'mywebapp-deploy-bucket'  // Replace with your actual bucket name
        APPLICATION_NAME = 'MyWebApp'
        DEPLOYMENT_GROUP_NAME = 'MY-DEPLOYMRNT-GROUP'
        AWS_REGION = 'ap-southeast-1' // Ensure the correct region is set
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/naikpradyumna295/Project-Oriserve.git'
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
                withAWS(credentials: 'aws-credentials', region: "${AWS_REGION}") {
                    sh "aws s3 cp MyWebApp.zip s3://${S3_BUCKET}/MyWebApp.zip"
                }
            }
        }
        stage('Deploy') {
            steps {
                withAWS(credentials: 'aws-credentials', region: "${AWS_REGION}") {
                    sh "aws deploy create-deployment --application-name ${APPLICATION_NAME} --deployment-config-name CodeDeployDefault.OneAtATime --deployment-group-name ${DEPLOYMENT_GROUP_NAME} --s3-location bucket=${S3_BUCKET},key=MyWebApp.zip,bundleType=zip"
                }
            }
        }
    }
}

