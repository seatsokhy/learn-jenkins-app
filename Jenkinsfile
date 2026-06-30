pipeline {
    agent{
            docker {
                image 'node:18-alpine' 
                reuseNode true
            }
        }
    stages {
        stage('build') {
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

        stage('pararell-running'){
             parallel {                 // parallel container
                stage('pararell start') {  // child stage 1
                    steps {
                        echo 'pararell starting'
                    }
                }
                stage('pararell start') {  // child stage 2
                    steps {
                        echo 'pararell Ending'
                    }
                }
            }
        }

    }
    post{
        always {
            junit 'test-results/junit.xml'
        }
    }
}