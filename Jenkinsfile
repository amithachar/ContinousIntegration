pipeline {
    agent any

    tools {
        jdk 'java-11'
        maven 'maven'
    }

    stages {
        stage('Git checkout') {
            steps {
                git branch: 'verion-1', url: 'https://github.com/ManojKRISHNAPPA/test-1.git'
            }
        }

        stage('Compile') {
            steps {
                sh 'mvn compile'
            }
        }		

        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Building-DockerImage') {
            steps {
                sh 'docker build -t manojkrishnappa/continous-intergartion:1 .'
            }
        }

        stage('Containersation') {
            steps {
                sh '''
                    docker stop c1 || true
                    docker rm c1 || true
                    docker run -it -d --name c1 -p 9000:8080 manojkrishnappa/continous-intergartion:1
                '''
            }
        }

        stage('Login to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', 
                                                      passwordVariable: 'DOCKER_PASSWORD', 
                                                      usernameVariable: 'DOCKER_USERNAME')]) {
                        sh 'echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin'
                    }
                }
            }
        }

        stage('Pushing Docker image to DockerHub') {
            steps {
                sh 'docker push manojkrishnappa/continous-intergartion:1'
            }
        }	
    }
}
