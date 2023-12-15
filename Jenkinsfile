pipeline {
    agent any
    stages {
        stage('Init') {
            steps {
                script{
                    if (env.GIT_BRANCH == "origin/main") {
                        sh '''
                        kubectl create namespace prod || echo "Namespace prod already exists"
                        '''
                    } else if (env.GIT_BRANCH == "origin/dev") {
                        sh '''
                        kubectl create namespace dev || echo "Namespace dev already exists"
                        '''
                    } else {
                        sh '''
                        echo "Branch not recognised"
                        '''
                    }
                }
            }
        }
        stage('Build') {
            steps {
                script {
                    if (env.GIT_BRANCH == "origin/main") {
                        sh '''
                        docker build -t arvindsiva68/proj3:latest -t arvindsiva68/proj3:prod-v${BUILD_NUMBER} .
                        '''
                    } else if (env.GIT_BRANCH == "origin/dev") {
                        sh '''
                        docker build -t arvindsiva68/proj3:latest -t arvindsiva68/proj3:dev-v${BUILD_NUMBER} .
                        '''
                    } else {
                        sh '''
                        echo "Branch not recognised"
                        '''
                    }
                }
            }
        }
        stage('Push') {
            steps {
                script{
                    if (env.GIT_BRANCH == "origin/main") {
                        sh '''
                        docker push arvindsiva68/proj3:latest
                        docker push arvindsiva68/proj3:prod-v${BUILD_NUMBER}
                        '''
                    } else if (env.GIT_BRANCH == "origin/dev") {
                        sh '''
                        docker push arvindsiva68/proj3:latest
                        docker push arvindsiva68/proj3:dev-v${BUILD_NUMBER}
                        '''
                    } else {
                        sh '''
                        echo "Branch not recognised"
                        '''
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                sh '''
                kubectl apply -f ./kubernetes
                kubectl set image deployment/flask-deployment task1=arvindsiva68/proj3:v${BUILD_NUMBER}
                '''
            }
        }

        stage('CleanUp') {
            steps {
                script{
                    if (env.GIT_BRANCH == "origin/main") {
                        sh '''
                        kubectl apply -n prod -f ./kubernetes
                        kubectl set image deployment/flask-deployment task1=arvindsiva68/proj3:prod-v${BUILD_NUMBER} -n prod
                        '''
                    } else if (env.GIT_BRANCH == "origin/dev") {
                        sh '''
                        kubectl apply -n dev -f ./kubernetes
                        kubectl set image deployment/flask-deployment task1=arvindsiva68/proj3:dev-v${BUILD_NUMBER} -n dev
                        '''
                    } else {
                        sh '''
                        echo "Branch not recognised"
                        '''
                    }
                }
            }
        }
    }
}