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
                    node --version
                    npm --version
                    
                    
                '''
            }
        }
        stage ('Test'){
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps{
                sh '''
                   echo Testing done
                '''
            }
        }
        stage ('E2E'){
            agent {
                docker {
                    image 'docker pull mcr.microsoft.com/playwright:v1.56.1-noble'
                    reuseNode true
                }
            }
            steps{
                sh '''
                   sudo npm install -g serve
                   serve -s build
                   npx playwright test
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