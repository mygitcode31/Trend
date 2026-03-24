pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "mydockerimage31/trend-app-image"
    }

    stages {

        stage('Clone Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/mygitcode31/Trend.git',
                    credentialsId: 'Git-Cred'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ${DOCKER_IMAGE}:${BUILD_NUMBER} .'
            }
        }

        stage('Login to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'Dockeruser', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh '''
                    echo $PASS | docker login -u $USER --password-stdin
                    '''
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                sh 'docker push ${DOCKER_IMAGE}:${BUILD_NUMBER}'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f service.yml'
                sh 'kubectl set image deployment/trend-deployment trend-app=${DOCKER_IMAGE}:${BUILD_NUMBER} --record'
            }
        }
    }
    
    post {
        always {
            cleanWs()
        }
    }

}
