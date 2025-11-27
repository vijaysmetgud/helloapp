pipeline {
    agent { label 'worker1' }

    options {
        skipDefaultCheckout()   // disable automatic Jenkins checkout
    }

    environment {
        DOCKERHUB_USER = "vsmetgud"
    }

    stages {

        stage('Checkout') {
            steps {
                git(
                    branch: 'main',
                    url: 'https://github.com/vijaysmetgud/helloapp.git'
                )
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKERHUB_USER/flask-app:latest .'
            }
        }
        
        stage('Push to DockerHub repo') {
            steps {
                 withCredentials([
                 usernamePassword(
                 credentialsId: 'dockerhub-pass',
                 usernameVariable: 'DOCKERHUB_USER',
                 passwordVariable: 'DOCKERHUB_PASS'
            )
        ]) {
            sh 'echo $DOCKERHUB_PASS | docker login -u $DOCKERHUB_USER --password-stdin'
            sh 'docker push $DOCKERHUB_USER/flask-app:latest'
        }
    }
}


        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl apply -f service.yaml'
            }
        }
    }
}
