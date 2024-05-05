pipeline {
    agent none
    stages {
        stage("Build Image") {
            agent {
                label "${env.BRANCH_NAME == 'test' ? 'container-slave' : env.BRANCH_NAME == 'master' ? 'ec2-slave' : 'any'}"
            }
            steps {
                script {
                    echo "Building the Docker image..."
                    withCredentials([usernamePassword(credentialsId: 'docker-creds', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                        sh 'sudo docker build -t aishafathy/node-app .'
                        sh "echo $PASS | sudo docker login -u $USER --password-stdin"
                    }
                }
            }
        }
        stage("Push Image") {
            agent {
                label "${env.BRANCH_NAME == 'test' ? 'container-slave' : env.BRANCH_NAME == 'master' ? 'ec2-slave' : 'any'}"
            }
            steps {
                script {
                    echo "Pushing the Docker image to Docker Hub..."
                    sh 'sudo docker push aishafathy/node-app'
                }
            }
        }
        stage('Run Docker Container') {
            agent {
                label "${env.BRANCH_NAME == 'test' ? 'container-slave' : env.BRANCH_NAME == 'master' ? 'ec2-slave' : 'any'}"
            }
            steps {
                script {
                    echo "Running node app Docker container..."
                    sh 'sudo docker pull aishafathy/node-app'
                    sh 'sudo docker run --name node-app -p 3000:3000 -d aishafathy/node-app' 
                }
            }
        }
    }
}
