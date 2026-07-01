pipeline {
    agent {
        docker {
            image 'node:18-alpine'
            reuseNode true
        }
    }

    environment {
        NETLIFY_SITE_ID = "3b040af5-3eb7-4a1b-89e2-45c5af938a62"
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
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
        stage('Approval'){

            steps {
                timeout(time: 30, unit: 'SECONDS') {
                    script {
                        input message: 'Build and Test are success. Deploy now?', ok: 'Yes'
                    }   
            }

        }
        stage('Deploy') {

            steps {
                sh '''
                    npm install netlify-cli@20.1.1

                    ./node_modules/.bin/netlify --version

                    echo "Deploy to Production with ID $NETLIFY_SITE_ID"

                    ./node_modules/.bin/netlify deploy --prod --dir=build
                '''
            }
        }

        stage('Parallel-Running') {
            parallel {

                stage('Parallel Start') {
                    steps {
                        echo 'Parallel starting'
                    }
                }

                stage('Parallel End') {
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