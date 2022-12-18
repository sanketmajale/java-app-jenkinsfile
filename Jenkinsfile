pipeline {
    agent { label 'google-vm' } 
    tools {
        maven 'M3'
    }
    environment {
        imageName = "java-login-app"
        dockerImage = ''
        dockerHubUser = 'sanketmajale'
    }
    stages {
        stage('Clone Repo') {
            steps {
                git 'https://github.com/sanketmajale/java-app-jenkinsfile.git'
            }
        
        }
        stage('Build MVN') {
            steps {
                sh 'mvn clean install'
            }
        
        }
        stage('Create Docker Image With Tag') {
            steps {
                script {
                   dockerImage = docker.build("${dockerHubUser}/${imageName}" ,"$WORKSPACE")  
                }
            }
        
        }
        stage('Push to DockerHub') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub', variable: 'dockerhub')]) {
                sh 'docker login -u ${dockerHubUser} -p ${dockerhub}'
            }
                
                sh 'docker push ${dockerHubUser}/${imageName}'
                
            }
        
        }
        /*stage('Deploy K8s Resorces') {
            steps {
                sh 'kubectl create -f $WORKSPACE/kubernetes-manifest/.'
                
            }
        }*/
    }
}
