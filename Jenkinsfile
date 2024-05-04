pipeline {
    agent any

    stages {
        stage('Code') {
            steps {
                echo 'Cloning the code'
                git url:"https://github.com/fasih6/notes-app-cicd.git", branch: "main"
            }
        }
        stage('Build') {
            steps {
                echo 'Building the image'
                sh "docker build -t my-notes-app ."
            }
        }
        stage('Push to docker hub') {
            steps {
                echo 'pushing the iomage to docker hub'
                withCredentials([usernamePassword(credentialsId:"dockerhubcred",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker tag my-notes-app ${env.dockerHubUser}/my-notes-app:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/my-notes-app:latest"
                }
                
            }
        }
        stage('deploy') {
            steps {
                echo 'Deploying the container'
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
