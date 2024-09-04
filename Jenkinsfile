pipeline {
    agent any
    environment {
        S3_BUCKET = 'mywebapp-deploy-bucket'  // Replace with your actual bucket name
        APPLICATION_NAME = 'MyWebApp'
        DEPLOYMENT_GROUP_NAME = 'MY-DEPLOYMENT-GROUP'
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
                withCredentials([awsCredentials(credentialsId: 'aws-credentials', accessKeyVariable: 'AWS_ACCESS_KEY_ID', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                    sh "aws s3 cp MyWebApp.zip s3://${S3_BUCKET}/MyWebApp.zip"
                }
            }
        }
        stage('Deploy') {
            steps {
                withCredentials([awsCredentials(credentialsId: 'aws-credentials', accessKeyVariable: 'AWS_ACCESS_KEY_ID', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                    sh "aws deploy create-deployment --application-name ${APPLICATION_NAME} --deployment-config-name CodeDeployDefault.OneAtATime --deployment-group-name ${DEPLOYMENT_GROUP_NAME} --s3-location bucket=${S3_BUCKET},key=MyWebApp.zip,bundleType=zip"
                }
            }
        }
    }
}
