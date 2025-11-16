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

        stage('Netlify') {
    steps {
        withDockerContainer(image: 'node:18-alpine', args: '-u 122:124') {
            sh '''
                # Create project-local npm cache directory
                mkdir -p /var/lib/jenkins/workspace/Learn-jenkins-Pipeline/.npm

                # Set npm cache locally (NOT globally)
                npm config set cache /var/lib/jenkins/workspace/Learn-jenkins-Pipeline/.npm

                # Install dependencies
                npm install
            '''
        }
    }
}


    }

    post {
        always {
            junit 'jest-results/junit.xml'
            publishHTML([
                allowMissing: false,
                alwaysLinkToLastBuild: false,
                keepAll: false,
                reportDir: 'playwright-report',
                reportFiles: 'index.html',
                reportName: 'Playwright HTML Report',
                reportTitles: '',
                useWrapperFileDirectly: true
            ])
        }
    }
}
