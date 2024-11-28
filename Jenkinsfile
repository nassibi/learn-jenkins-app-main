pipeline {
    agent any



    stages {
        stage('Fix npm Permissions') {
            steps {
                script {
                    // Fix the ownership of npm cache directory inside the container
                    sh 'sudo chown -R 979:977 /.npm'
                    sh 'sudo chown -R $(whoami):$(whoami) /.npm'
                }
            }
        }
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    ls -la
                    node --version
                    npm --version                    
                    npm install
                    npm run build
                    ls -la
                '''
            }
        }
    }
}
