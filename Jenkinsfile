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
                    // Create a custom npm cache directory inside the workspace
                    sh '''
                        # Create a custom npm cache directory inside the workspace to avoid permission issues
                        mkdir -p /var/lib/jenkins/workspace/learn-jenkins-app-main/.npm

                        # Set npm to use this directory for its cache
                        npm config set cache /var/lib/jenkins/workspace/learn-jenkins-app-main/.npm --global

                        # Optionally, set a custom directory for npm's user config (to avoid global directories)
                        npm config set prefix /var/lib/jenkins/workspace/learn-jenkins-app-main/npm-global --global

                        # Ensure the custom cache directory is writable
                        chown -R node:node /var/lib/jenkins/workspace/learn-jenkins-app-main/.npm
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
