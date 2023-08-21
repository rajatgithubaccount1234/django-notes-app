pipeline {
    agent any

    stages {
        stage('checkout the code from SCM') {
            steps {
                echo 'Cloning the code from Github'
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'usingsshnotes', url: 'https://github.com/rajatgithubaccount1234/django-notes-app.git']])
            }
        }
        stage('Build') {
            steps {
                echo 'Building the code'
                sh 'docker build -t notes-app .'
                sh 'docker image ls'
            }
        }
        stage('Push to Docker Hub') {
            steps {
                echo 'Pushing Image to docker Hub'
                withCredentials([usernamePassword(credentialsId: 'realdockerhubpassword', passwordVariable: 'realdockerhubpassword',usernameVariable: 'dockerHubUser')]) {
                           // some block
                sh "docker tag notes-app ${env.dockerHubUser}/my-notes-app:latest1"           
                sh "docker login -u ${env.dockerHubUser} -p ${env.realdockerhubpassword}" 
                sh "docker push ${env.dockerHubUser}/my-notes-app:latest1"
                }
                
                      
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying image to container'
                sh 'pwd' 
                
                sh "docker-compose up -d"
            }
        }
    }
}
