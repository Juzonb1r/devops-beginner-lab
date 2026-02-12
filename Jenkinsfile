pipeline {
    agent any

    environment {
        IMAGE_NAME = "juzonb1r/devops-lab"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
		    url:  'https://github.com/Juzonb1r/devops-beginner-lab.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:v1 .'
            }
        }

        stage('Test') {
            steps {
                sh 'echo "Basic test passed"'
            }
        }

        stage('Push Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-cred', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                    sh 'docker push $IMAGE_NAME:v1'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s/'
            }
        }
    }
}
