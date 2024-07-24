pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh '''
                docker build -t zaiyad/duo-deploy-flask:latest -t zaiyad/duo-deploy-flask:v$BUILD_NUMBER .
                '''
            }
        }
        stage('Push') {
            steps {
                sh '''
                docker push zaiyad/duo-deploy-flask:latest
                docker push zaiyad/duo-deploy-flask:v$BUILD_NUMBER
                '''
            }
        }
        stage('Cleanup') {
            steps {
                sh '''
                docker rmi zaiyad/duo-deploy-flask:latest
                docker rmi zaiyad/duo-deploy-flask:v$BUILD_NUMBER
                ''' 
            } 
        }
        stage('Deploy') {
            steps {
                sh '''
                kubectl apply -f ./k8s-deployments
                kubectl rollout restart deployment flask-deployment
                '''
            }
        }
    }
}
