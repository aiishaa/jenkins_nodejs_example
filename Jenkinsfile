pipeline {
    agent {
        label "${env.BRANCH_NAME == 'test' ? 'container-slave' : env.BRANCH_NAME == 'master' ? 'ec2-slave' : 'any'}"
    }
    stages {
        stage("Build Image") {
            steps {
                script {
                    echo "lsit contents'
                    sh 'ls'
                    }
                }
            }
        }
    }
}
