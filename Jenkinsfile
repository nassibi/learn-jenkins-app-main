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
                    // Create a custom npm cache directory inside the Jenkins workspace
                    sh '''
                        # Create a custom npm cache directory inside the workspace to avoid permission issues
                        mkdir -p /var/lib/jenkins/workspace/learn-jenkins-app-main/.npm
                        npm config set cache /var/lib/jenkins/workspace/learn-jenkins-app-main/.npm --global

                        # Fix permissions of the custom npm cache directory (since the container runs as root)
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
