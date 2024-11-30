pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:23'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    # Create a local npmrc to avoid using global directories
                    # echo "prefix=/var/lib/jenkins/workspace/learn-jenkins-app-main/npm-global" > .npmrc
                    # echo "cache=/var/lib/jenkins/workspace/learn-jenkins-app-main/.npm" >> .npmrc
                    
                    # Run npm install and build
                    npm ci
                    npm run build
                '''
            }
        }
    }
}
