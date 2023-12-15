pipeline {
    agent any
    environment {
        PROJECT_ID = ' [PROJECT_ID_GOES_HERE] '
        CLUSTER_NAME = 'demo-cluster'
        LOCATION = 'europe-west2-c'
        CREDENTIALS_ID = '[CREDENTIALS_ID_GOES_HERE(for the k8s service account)]'
    }

    stages {
        stage('Build and Push to Docker') {
            steps {
                script {
                    if (env.GIT_BRANCH == "origin/main") {
                        sh '''
                        docker build -t arvindsiva68/flask-jenk:latest -t arvindsiva68/flask-jenk:prod-v${BUILD_NUMBER} .
                        '''
                    } else {
                        sh '''
                        echo "Branch not recognised"
                        '''                       
                    }

                }

            }
        }
        stage('Deploy to GKE') {
            steps {
                script {
                    // Deploy to GKE using Jenkins Kubernetes Engine Plugin
                    step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME, location: env.LOCATION, manifestPattern: 'kubernetes/deployment.yaml', credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])
                }
            }
        }
    }
}