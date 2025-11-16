pipeline {
    agent {
        docker {
            image 'node:18-alpine'
            args '-u 122:124'       // same user for entire pipeline
            reuseNode true
        }
    }

    stages {

        stage('Build') {
            steps {
                sh '''
                    cd $WORKSPACE

                    # Ensure workspace is owned by our Docker user
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

                    # Fix workspace ownership again
                    chown -R 122:124 .

                    # Force npm to use a local user-level config file
                    export NPM_CONFIG_USERCONFIG=$WORKSPACE/.npmrc

                    # Create local npm cache
                    mkdir -p $WORKSPACE/.npm
                    echo "cache=$WORKSPACE/.npm" > $WORKSPACE/.npmrc

                    echo "===== NPM CONFIG ====="
                    npm config list

                    echo "===== INSTALLING DEPENDENCIES ====="
                    npm install
                    npm audit fix

                    # If you want to build:
                    # npm run build

                    # If deploying to Netlify:
                    # npx netlify deploy --auth "$NETLIFY_AUTH_TOKEN" --prod
                '''
            }
        }
    }

    post {
        always {
            // Disable JUnit because your project does NOT generate junit.xml
            // junit testResults: 'jest-results/junit.xml', allowEmptyResults: true

            publishHTML([
                allowMissing: true,
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
