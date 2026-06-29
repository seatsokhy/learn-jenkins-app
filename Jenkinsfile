pipeline {
    agent any

    stages {
        stage('build') {
            agent{
                docker {
                    image 'node:18-alpine' 
                    reuseNode true
                }
            }
            steps {
                sh '''
                    node --version
                    npm --version
                    npm ci
                    npm run build
                '''
            }
        }
        stage('Test'){
            steps {
                sh '''
                    echo "Start Testing =========>"
                    npm test -- --watchAll=false
                    echo "End Testing"
                '''
            }
        }
    }
}