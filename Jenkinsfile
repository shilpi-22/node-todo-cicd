pipeline {
    agent any
  environment {
        LOGIN_CREDS = credentials('dockerhub')
    }
    stages {
        stage('Code') {
            steps {
                echo 'Clone Code'
                git url: 'https://github.com/shilpi-22/node-todo-cicd.git', branch: 'master'
            }
        }
        stage('Build'){
            steps{
                echo 'Build Code'
                sh "docker build . -t node-todo-app"
            }
        }
        stage('Push'){
            steps{
               
                withCredentials([usernamePassword(credentialsId:"dockerhub", passwordVariable:"dockerHubPass",
                usernameVariable:"dockerHubUser")]){
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker tag node-todo-app ${env.dockerHubUser}/node-todo-app:latest"
                   echo "Login Success"
                    sh "docker push ${env.dockerHubUser}/node-todo-app:latest"
                
                }
            }
        }
        stage('Deploy'){
            steps{
                echo 'Deploy code'
                sh "docker run -d -p 8000:8000 node-todo-app:latest"
                
            }
        }
    }
}
