pipeline {
    agent {label ec2-slave }
    stages {
        stage("build image") {
            steps {
                script {
                    echo "building the docker image..."
                    withCredentials([usernamePassword(credentialsId: 'docker-creds', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                      sh 'docker build -t aishafathy/node-app .'
                      sh "echo $PASS | docker login -u $USER --password-stdin"
                    }
                }
            }
        }
        stage("push image") {
            steps {
                script {
                    echo "Push the docker image to docker hub..."
                    sh 'docker push aishafathy/node-app'
                }
            }
        }
      stage('Run Docker Container') {
          steps {
            script {
                echo "Running node app docker container..."
                sh 'docker pull aishafathy/node-app'
                sh 'docker run --name node-app -p 3000:3000 aishafathy/node-app' 
            }
          }
      }
    }
}
