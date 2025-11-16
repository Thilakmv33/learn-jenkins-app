pipeline {
    agent {
        docker {
            image 'node:18-alpine'
            args '-u 122:124'          // Use same user in ALL stages
            reuseNode true
        }
    }

    stages {

        stage('Build') {
            steps {
                sh '''
                    cd $WORKSPACE

                    # Fix ownership
                    chown -R 122:124 .

                    node --version
                    npm --version
                    ls -la
                '''
            }
        }

        stage('Netlify') {
            steps {
                sh '''
                    cd $WORKSPACE

                    # Fix workspace permissions
                    chown -R 122:124 .

                    # Create local npm cache
                    mkdir -p .npm

                    # Set npm cache (LOCAL -- not global)
                    npm config set cache $(pwd)/.npm

                    # Install dependencies
                    npm install

                    # Run your build or publish steps
                    # npm run build
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
