pipeline {
    agent any

    environment {
        DOCKERHB_CREDENTIALS = credentials('dockerhub')
    }

    stages {
        stage('Build and Test') {
            steps {
                sh 'docker build . -t dinhcam89/node-todo-test:latest'
            }
        }
        stage('Login') {
            steps {
                sh '''
                    echo "$DOCKERHB_CREDENTIALS_PSW" |
                    echo "$DOCKERHB_CREDENTIALS_USR" |
                    docker login -u $DOCKERHB_CREDENTIALS_USR -p $DOCKERHB_CREDENTIALS_PSW
                '''
            }
        }
        stage('View Images') {
            steps {
                sh 'docker images'
            }
        }
        //cmt234
        stage('Docker Tag') {
            steps {
                sh 'docker tag node-todo-test node-todo-test:latest'
            }
        }
        stage('Push') {
            // some block
            steps {
                sh 'docker push dinhcam89/node-todo-test:latest'
            }
        }
        stage('Deploy to EKS Cluster') {
            steps {
                sh 'kubectl apply -f deployment.yml'
            }
        }
    }
    post {
        always {
            sh 'docker logout'
        }
    }
}
