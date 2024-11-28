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
                script {
                    // Set up custom npm cache and user directory to avoid permission issues
                    sh '''
                        # Create a custom npm cache directory inside the workspace
                        mkdir -p /var/lib/jenkins/workspace/learn-jenkins-app-main/.npm
                        
                        # Set npm to use this directory for its cache and other directories
                        npm config set cache /var/lib/jenkins/workspace/learn-jenkins-app-main/.npm --userconfig /var/lib/jenkins/workspace/learn-jenkins-app-main/.npmrc
                        npm config set prefix /var/lib/jenkins/workspace/learn-jenkins-app-main/npm-global --userconfig /var/lib/jenkins/workspace/learn-jenkins-app-main/.npmrc
                        
                        # Ensure the custom directories are writable
                        chown -R node:node /var/lib/jenkins/workspace/learn-jenkins-app-main/.npm
                        chown -R node:node /var/lib/jenkins/workspace/learn-jenkins-app-main/npm-global
                    '''
                }
                sh '''
                    # Print file listings to confirm everything is correct
                    ls -la
                    node --version
                    npm --version

                    # Install dependencies with npm ci
                    npm ci

                    # Run the build process
                    npm run build

                    # Print file listings after the build
                    ls -la
                '''
            }
        }
    }
}
