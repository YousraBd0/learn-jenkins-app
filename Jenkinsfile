
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
                    echo "Starting React application build..."
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -la
                    echo "Build completed successfully!"
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
                    echo "Starting tests..."
                    test -f build/index.html
                    CI=true npm test -- --watchAll=false
                    echo "Tests completed successfully!"
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
                    npm install netlify-cli -g   
                    node_modules/.bin/netlify --version 
                '''
            }
        }
    }

    post {
        always {
            junit 'test-results/junit.xml'
        }

        
    }
}
