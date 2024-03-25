pipeline {
    agent any    

    tools {
        maven "Maven-3-9-6"
    }
        
    stages {
        stage("Maven Build") {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/PrashanthNander/springboot-docker']])
                bat 'mvn -B -DskipTests clean package'
            }
        }
		
        stage("Maven Test") {
            steps {
                bat 'mvn test'
            }
            post {
                always {
                    junit 'target/junit-reports/*.xml'
                }
            }
        } 
        
        stage("Build Docker Image") {
            steps {
                script {
                    bat 'docker build -t pnander/springboot-docker:1.0 .'
                }
            }
        }
        
        stage ("Push Image to Docker Hub") {
            steps {
                script {
                    withCredentials([string(credentialsId: 'DockerHubPwd', variable: 'dockerhub-pwd')]) {
                        bat 'docker login -u pnander -p %dockerhub-pwd%'
                    }
                    bat 'docker push pnander/springboot-docker:1.0'
                }
            }
        }
    }
}
