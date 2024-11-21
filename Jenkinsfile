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

        stage('Build and run docker image') {
            steps {
                sh 'sudo docker build -t joamal/myapp:v1 .'
                sh "sudo docker tag joamal/myapp:v1 joamal/myapp:develop-${env.BUILD_ID}" 
                sh 'sudo docker run -d -p 8081:80 joamal/myapp:v1'
            } 
        }


        stage('Build and Push') {
            steps {
                echo 'Building..'
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-auth', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                        sh '''
                            sudo docker login -u ${USERNAME} -p ${PASSWORD}
                            sudo docker push joamal/myapp:v1
                        '''
                        sh "sudo docker push joamal/myapp:develop-${env.BUILD_ID}"
                    }
            }
        }

        stage('testing') {
            steps {
                sh 'curl -I 43.204.234.185:8081'
            }
        }

    
    }
}
