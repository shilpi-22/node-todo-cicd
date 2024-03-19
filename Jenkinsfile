pipeline {
    agent any

    stages {
        stage('Code') {
            steps {
                git url: "https://github.com/shilpi-22/node-todo-cicd.git", branch: "master"
                echo "Code clone successful"
            }
        }
          stage('Build') {
            steps {
                sh "docker build . -t node-todo-cicd"
                echo "Code Build successful"
            }
        }
        
        stage("Push to Private Docker Hub Repo") {
            steps {
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerPass",
                usernameVariable:"dockerUser")]) {
                    sh "docker login -u ${env.dockerUser} -p ${env.dockerPass}"
                    sh "docker tag node-todo-cicd:latest ${env.dockerUser}/node-todo-cicd:latest" // tag docker image to updated dockerimage with username of dockerHub
                    sh "docker push ${env.dockerUser}/node-todo-cicd:latest" // Push the updated docker image name to dockerHub
                    echo "Docker Image Push successful"
                }
            }
        }

        stage("deploy") {
            steps {
                sh "docker-compose down" //put down the running containers
                sh "docker-compose up -d" // Make sure docker-compose is installed and config is correct
                echo "Node-app deployment successful"
            }
        }
    }
}
