pipeline {
    agent any
    environment {
        REGISTRY = 'registry.example.com'
        IMAGE = "${REGISTRY}/app:${BUILD_NUMBER}"
    }
    stages {
        stage('Checkout') {
            steps { checkout scm }
        }
        stage('Build') {
            steps {
                sh 'docker build -t ${IMAGE} .'
                sh 'docker push ${IMAGE}'
            }
        }
        stage('Test') {
            steps {
                sh 'pip install -r requirements.txt'
                sh 'pytest tests/ -v --junitxml=results.xml'
            }
            post {
                always {
                    junit 'results.xml'
                }
            }
        }
        stage('Deploy Staging') {
            when { branch 'develop' }
            steps {
                sh 'kubectl set image deployment/app app=${IMAGE} -n staging'
                sh 'kubectl rollout status deployment/app -n staging'
            }
        }
        stage('Deploy Production') {
            when { branch 'main' }
            input { message 'Deploy to production?' }
            steps {
                sh 'kubectl set image deployment/app app=${IMAGE} -n production'
                sh 'kubectl rollout status deployment/app -n production'
            }
        }
    }
    post {
        failure { slackSend channel: '#alerts', message: "Build failed: ${JOB_NAME} #${BUILD_NUMBER}" }
        success { slackSend channel: '#deployments', message: "Deployed ${IMAGE} successfully" }
    }
}
