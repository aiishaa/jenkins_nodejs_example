pipeline {
    agent none
    stages {
        stage("build image") {
            agent {
                if (env.BRANCH_NAME == 'test') {
                    label 'container-slave'
                } else if (env.BRANCH_NAME == 'master') {
                    label 'ec2-slave'
                }
            }
            steps {
                script {
                    echo "building the docker image..."
                    withCredentials([usernamePassword(credentialsId: 'docker-creds', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                      sh 'sudo docker build -t aishafathy/node-app .'
                      sh "echo $PASS | sudo docker login -u $USER --password-stdin"
                    }
                }
            }
        }
        stage("push image") {
            agent {
                if (env.BRANCH_NAME == 'test') {
                    label 'container-slave'
                } else if (env.BRANCH_NAME == 'master') {
                    label 'ec2-slave'
                }
            }
            steps {
                script {
                    echo "Push the docker image to docker hub..."
                    sh 'sudo docker push aishafathy/node-app'
                }
            }
        }
        stage('Run Docker Container') {
            agent {
                if (env.BRANCH_NAME == 'test') {
                    label 'container-slave'
                } else if (env.BRANCH_NAME == 'master') {
                    label 'ec2-slave'
                }
            }
            steps {
                script {
                    echo "Running node app docker container..."
                    sh 'sudo docker pull aishafathy/node-app'
                    sh 'sudo docker run --name node-app -p 3000:3000 aishafathy/node-app' 
                }
              }
          }
        }
    }
