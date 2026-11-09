
pipeline {
    agent any

    enivironment {
        NETLIFY_SITE_ID = 'd20eab00-af77-4b16-9dab-00f0f23e288e'
    }

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

        stage('Tests') {
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

            post {
        always {
            junit 'test-results/junit.xml'
        }

        
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
