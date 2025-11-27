pipeline {
    agent { label 'worker1' }

    environment {
        DOCKERHUB_USER = "vsmetgud"
    }

    stages {
        stage('Checkout') {
            steps { git 'https://github.com/vijaysmetgud/helloapp.git' }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKERHUB_USER/flask-app:latest .'
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub-pass', variable: 'PASS')]) {
                    sh 'echo $PASS | docker login -u $DOCKERHUB_USER --password-stdin'
                    sh 'docker push $DOCKERHUB_USER/flask-app:latest'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s-deployment.yaml'
                sh 'kubectl apply -f k8s-service.yaml'
            }
        }
    }
}
