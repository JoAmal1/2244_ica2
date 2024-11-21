pipeline {
    agent any
    stages {
        stage('Cleanup') {
            steps {
                cleanWs()
            }
        }

        stage('Clone Git Repo') {
            steps {
                checkout scm
            }
        }
        stage('Clone from repository') {
            steps {
                git url: 'https://github.com/JoAmal1/2244_ica2.git', branch: 'develop', credentialsId: 'GIT'
            }
        }

        stage('Build and Push') {
            steps {
                echo 'Building..'
                dir('app'){
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-auth', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                        sh '''
                            docker build -t joamal/myapp:v1 .
                            docker login -u ${USERNAME} -p ${PASSWORD}
                            docker push joamal/myapp:v1
                        '''
                    }
                }
            }
        }

        stage('Deploy container'){
            steps {
                echo "deploying container"
                sh 'docker stop myapp || true && docker rm myapp || true'
                sh 'docker run --name myapp -d -p 8081:80 joamal/myapp:v1'
            }
        }
        stage('testing') {
            steps {
                sh 'curl -I 43.204.234.185:8081'
            }
        }

    
    }
}
