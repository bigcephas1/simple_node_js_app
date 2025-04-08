pipeline {
    agent any

    environment {
        IMAGE_NAME = "peterukpabi4/ci-cd-pipeline-app"
    }

    stages {
        stage('Clone Repo') {
            steps {
                git 'https://github.com/bigcephas1/simple_node_js_app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t $IMAGE_NAME .'
                }
            }
        }

        stage('Login to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKERHUB_USER', passwordVariable: 'DOCKERHUB_PASS')]) {
                    sh 'echo $DOCKERHUB_PASS | docker login -u $DOCKERHUB_USER --password-stdin'
                }
            }
        }

        stage('Push Image to DockerHub') {
            steps {
                sh 'docker push $IMAGE_NAME'
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                docker rm -f ci-cd-container || true
                docker run -d --name ci-cd-container -p 3000:3000 $IMAGE_NAME
                '''
            }
        }
    }
}

