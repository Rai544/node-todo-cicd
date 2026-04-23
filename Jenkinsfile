pipeline {
    agent { label "rai"}

    stages {
        stage('Code') {
            steps {
                echo 'This is cloning the code'
                git url:"https://github.com/Rai544/node-todo-cicd.git", branch:"main"
                echo "code cloned successfully"
            }
        }
        stage('Build') {
            steps {
                echo 'This is building the code'
                sh "docker build -t node-app ."
                echo "docker images builds."
            }
        }
        stage('login, taging, and pushing') {
            steps {
                echo 'This is loging the docker'
                withCredentials([usernamePassword('credentialsId':"dockerHubCred", passwordVariable:"dockerHubPass", usernameVariable:"dockerHubUser")]){
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                echo "docker login successfully"
                sh "docker tag node-app:latest ${env.dockerHubUser}/node-app:latest"
                echo "image is taged"
                sh "docker push ${env.dockerHubUser}/node-app:latest"
                }
            }
        }
        stage('Test') {
            steps {
                echo 'This is testing the code'
            }
        }
        stage('Deploy') {
            steps {
                echo 'This is deploying the code'
                sh "docker compose down"
                echo "container is down"
                sh "docker compose up -d"
                echo "docker container is runing"
            }
        }
    }
}
