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
                sh '''
                    ls -la
                    npm cache clean --force
                    rm -rf node_modules package-lock.json
                    
                    # Set npm config for stability
                    npm config set fetch-retries 5
                    npm config set fetch-timeout 300000
                    
                    # Try install with fallback
                    npm install || npm install --legacy-peer-deps
                    
                    # Build
                    npm run build
                '''
            }
        }
        stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    test -f build/index.html
                    npm test
                '''
            }
        }
        stage('Deploy') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                   npm install netlify-cli 
                   node_modules/.bin/netlify --version
                '''
            }
        }
    }
}
