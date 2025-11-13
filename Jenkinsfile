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
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    # Create writable npm directories
                    mkdir -p /tmp/npm-global
                    npm config set prefix /tmp/npm-global
                    npm config set cache /tmp/.npm-cache --global

                    # Install Netlify CLI globally (in /tmp)
                    npm install -g netlify-cli

                    # Add the npm global bin to PATH
                    export PATH=/tmp/npm-global/bin:$PATH

                    # Verify installation
                    netlify --version
                '''
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
