pipeline {
    agent {
        docker {
            image 'node:18-alpine'
            reuseNode true
        }
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
                    install netlify-cli@20.1.1
                    node_modules\.bin\netlify --version
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