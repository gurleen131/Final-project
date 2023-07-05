pipeline {
    agent any

    stages {
        stage('authentication') {
            steps {
                sh 'echo authenticating'
            }
        }
        stage('build'){
            steps{
                sh 'echo building...'
            }
        }
        stage('push to ECR'){
            steps{
                sh 'echo pushing...'
            }
        }
    }

}