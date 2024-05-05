pipeline {
    agent {
        label "${env.BRANCH_NAME == 'test' ? 'container-slave' : env.BRANCH_NAME == 'master' ? 'ec2-slave' : 'any'}"
    }
    stages {
        stage("Build Image") {
            steps {
                script {
                    echo "Building the Docker image..."
                    withCredentials([usernamePassword(credentialsId: 'docker-creds', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                        sh 'docker build -t aishafathy/node-app .'
                        sh "echo $PASS | docker login -u $USER --password-stdin"
                    }
                }
            }
        }
        stage("Push Image") {
            steps {
                script {
                    echo "Pushing the Docker image to Docker Hub..."
                    sh 'docker push aishafathy/node-app'
                }
            }
        }
        stage('Run Docker Container') {
            steps {
                script {
                    echo "Running node app Docker container..."
                    sh 'docker pull aishafathy/node-app'
                    sh 'docker run --name node-app -p 3000:3000 -d aishafathy/node-app' 
                }
            }
        }
    }
}
