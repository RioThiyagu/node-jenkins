pipeline {
    agent any
    stages {
        stage('Tests') {
            steps {
//                 script {
//                    docker.image('node:10-stretch').inside { c ->
                        echo 'Building..'
                        sh 'npm install'
                        echo 'Testing..'
                        sh 'npm test'
//                         sh "docker logs ${c.id}"
//                    }
//                 }
            }
        }
        stage('Build and push docker image') {
            steps {
                script {
                    def dockerImage = docker.build("thiyagurio/node-demo:master")
                    docker.withRegistry('', 'docker-demo') {
                        dockerImage.push('master')
                    }
                }
            }
        }
        stage('Deploy to remote docker host') {
            environment {
                DOCKER_HOST_CREDENTIALS = credentials('docker-demo')
            }
            steps {
                script {
//                  sh 'docker login -u $thiyagurio -p $docker-demo 54.152.228.229'
                    sh 'docker rm -f node-demo:master'
                    sh 'docker pull thiyagurio/node-demo:master'
                    sh 'docker tag thiyagurio/node-demo:master thiyagurio/node-demo:master'
                    sh 'docker run -d --name node-demo -p 80:3000 thiyagurio/node-demo:master'
                }
            }
        }
    }
}
