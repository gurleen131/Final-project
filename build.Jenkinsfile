pipeline {
    agent any

    environment {
        ECR_URL = '854171615125.dkr.ecr.us-east-2.amazonaws.com'
        REPO_NAME = 'gurleen-jenkins'
    }

    stages {
        stage('ECR authentication and Docker login') {
            steps {

                sh '''
                aws ecr get-login-password --region us-east-2 | docker login --username AWS --password-stdin 854171615125.dkr.ecr.us-east-2.amazonaws.com
                #cd yolo5
                #docker build -t gurleen-jenkins .
                #docker tag gurleen-jenkins:latest 854171615125.dkr.ecr.us-east-2.amazonaws.com/gurleen-jenkins:latest
                #docker push 854171615125.dkr.ecr.us-east-2.amazonaws.com/gurleen-jenkins:latest
                '''
            }
        }

        stage('Build') {
            steps {
                sh '''
                cd yolo5
                docker build -t ${REPO_NAME} .
                '''
            }
        }

        stage('Push to ECR') {
            steps {
                sh '''
                docker tag ${REPO_NAME} ${ECR_URL}/${REPO_NAME}:0.0.${BUILD_NUMBER}
                docker push ${ECR_URL}/${REPO_NAME}:0.0.${BUILD_NUMBER}
                '''
            }
        }

        stage('Trigger Deploy') {
            steps {
                build job: 'Yolo5Deploy', wait: 'false' , parameters: [
                    string(name: 'YOLO5_IMAGE_URL', value: "${ECR_URL}/${REPO_NAME}:0.0.${BUILD_NUMBER}")
                    ]
            }
        }
    }
}