pipeline {
    agent any

    stages {
        stage('code') {
            steps {
                echo 'code is getting from git'
                git url:'https://github.com/mastersofterp/JenkinProject.git/', branch:'main'
            }
        }
        stage('build') {
            steps {
                echo 'code is build'
                sh 'docker build -t my-notes-app .'
            }
        }
        stage('Image push to dockerHub') {
            steps {
                echo 'Pushing the image to dockerHub'
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker tag my-notes-app ${env.dockerHubUser}/my-notes-app:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/my-notes-app:latest"
                }
            }
        }
        stage('Deploy') {
            steps {
                echo 'deploying the code'
                sh 'docker run -d -p 8000:8000 mastersofterp/my-notes-app:latest'
            }
        }
    }
    
}
