pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                script {
                    // Fix the ownership of npm cache directory inside the container
                    sh '''
                        # Fix ownership of the npm cache directory to avoid EACCES issues
                        sudo chown -R $(whoami):$(whoami) /.npm
                        # Alternatively, use a custom cache directory
                        npm config set cache /var/lib/jenkins/workspace/learn-jenkins-app-main/.npm --global
                    '''
                }
                sh '''
                    # Print the file listing and check versions
                    ls -la
                    node --version
                    npm --version

                    # Run npm install with the clean state
                    npm ci

                    # Run the build process (ensure it runs successfully)
                    npm run build

                    # Print file listing after the build
                    ls -la
                '''
            }
        }
    }
}
