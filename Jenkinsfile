pipeline {
    agent any
    stages {
        stage('Init') {
            steps {
                sh '''
                ssh -i ~/.ssh/id_rsa jenkins@10.200.0.13 << EOF
                docker stop flask-app || echo "flask-app not running"
                docker rm flask-app || echo "flask-app not running"
                '''
           }
        }
        stage('Build') {
            steps {
                sh '''
                docker build -t arvindsiva68/python-api -t arvindsiva68/python-api:v${BUILD_NUMBER} .
                '''
           }
        }
        stage('Push') {
            steps {
                sh '''
                docker push arvindsiva68/python-api
                docker push arvindsiva68/python-api:v${BUILD_NUMBER}
                '''
           }
        }
        stage('Deploy') {
            steps {
                sh '''
                ssh -i ~/.ssh/id_rsa jenkins@10.200.0.13 << EOF
                docker run -d -p 80:8080 --name flask-app arvindsiva68/python-api
                '''
           }
        }
        stage('Cleanup') {
            steps {
                sh '''
                docker system prune -f
                '''
            }
        }
    }
}
