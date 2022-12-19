pipeline {
    agent { label 'google-vm' } 
    tools {
        maven 'M3'
    }
    environment {
        imageName = "java-login-app"
        dockerImage = ''
        dockerHubUser = 'sanketmajale'
        DOCKERHUB_CREDENTIALS=credentials('docker-registery')
    }
    stages {
        stage('git clone') {
            steps {
                git branch: 'main',
                    credentialsId: '9fe56b8e-d059-4cc3-80d9-d2b76cbef6a9',
                    url: 'https://github.com/sanketmajale/java-app-jenkinsfile.git'
               }
        }
        stage('Build MVN') {
            steps {
                sh 'mvn clean install'
            }
        
        }
        stage('Create Docker Image With Tag') {
            steps {
                sh 'docker build -t sanketmajale/app-image-jenkins:1 .'
            }
        
        }
        stage('Push to DockerHub') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'        
            
                sh 'docker push sanketmajale/app-image-jenkins:1'
            }
        
        }
        stage('Deploy K8s Resorces') {
            steps {
                sh 'kubectl run pod jenkins-pipline --image=nginx --port=80 '    
            }
        }
    }
}
