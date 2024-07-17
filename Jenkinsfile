pipeline {
    agent {label 'dev'}
    stages{
        stage('Code'){
            steps {
                script{
                    properties([pipelineTriggers([pollSCM('')])])
                }
                git url:'https://github.com/GMATHUR90/node-todo-cicd.git', branch: 'master'
            }
        }
        stage('Build and Test'){
            steps{
                script {
                    sh 'docker build . -t gmathur90/node-todo-app-cicd:latest'
                }
            }
        }
        stage('Login and Push Image'){
            steps{
                echo "Login to docker and Push Image"
            withCredentials([usernamePassword(credentialsId:'dockerhub',passwordVariable:'dockerHubPassword',usernameVariable:'dockerHubUsername')])
            {
                sh "docker login -u ${env.dockerHubUsername} -p ${env.dockerHubPassword}"
                sh "docker push gmathur90/node-todo-app-cicd:latest"
            }
            
            }
                    }
        stage('Deploy'){
            steps{
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
