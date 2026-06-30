pipeline {
    agent {
        docker {
            image 'node:18-alpine'
            reuseNode true
        }
    }
    environment{
        NETLIFY_SITE_ID="hello this is a secrect you can know it!!"
    }

    stages {
        stage('Build') {
            steps {
                sh '''
                    node --version
                    npm --version
                    npm ci
                    npm run build
                '''
            }
        }

        stage('Test') {
            steps {
                sh '''
                    echo "Start Testing =========>"
                    npm test -- --watchAll=false
                    echo "End Testing"
                '''
            }
        }

        stage('deploy') {
            steps {
                sh '''
                   npm install netlify-cli@20.1.1
                   ./node_modules/.bin/netlify --version
                   echo "Deploy to Production with ID $NETLIFY_SITE_ID"
                '''
            }
        }


        stage('Parallel-Running') {
            parallel {                 // parallel container
                stage('Parallel Start') {  // child stage 1
                    steps {
                        echo 'Parallel starting'
                    }
                }
                stage('Parallel End') {  // child stage 2
                    steps {
                        echo 'Parallel ending'
                    }
                }
            }
        }
    }

    post {
        always {
            junit 'test-results/junit.xml'
        }
    }
}