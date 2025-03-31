pipeline {
    agent any

    environment{
        NETLIFY_SITE_ID = 'bbbbf7a3-23e9-4215-b9fe-9a7ea3cc4045'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
    }
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
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }

        stage('Run Tests') {
            parallel{
                stage('Unit Test'){
                    agent {
                        docker {
                            image 'node:18-alpine'
                            reuseNode true
                        }
                    }
                    steps{
                        sh '''
                            test -f build/index.html
                            npm test
                        '''
                    }
                    post{
                        always {
                            junit 'jest-results/junit.xml'
                        }
                    }         
                }

                stage('E2E'){
                    agent{
                        docker {
                            image 'mcr.microsoft.com/playwright:v1.51.1-noble'
                            reuseNode true
                        }
                    }
                    steps{
                        echo "E2E"
                        sh '''
                            npm install serve
                            node_modules/.bin/serve -s build &
                            sleep 10
                            npx playwright install
                            npx playwright test --reporter=html 
                        '''
                    }
                    post{
                        always {
                            publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, icon: '', keepAll: false, reportDir: 'playwright-report', reportFiles: 'index.html', reportName: 'PlayRight Test Report', reportTitles: '', useWrapperFileDirectly: true])
                        }
                    }
                }
            }
        }


        stage('Deploy Staging'){
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true                
                }
            }
            steps{
                sh '''
                    npm install netlify-cli
                    node_modules/.bin/netlify --version
                    echo "Deploying into NETLIFY_SITE_ID = ${NETLIFY_SITE_ID}"
                    node_modules/.bin/netlify status
                    node_modules/.bin/netlify deploy --dir=build

                '''
            }
        }

        stage('Deploy Prod'){
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true                
                }
            }
            steps{
                sh '''
                    npm install netlify-cli
                    node_modules/.bin/netlify --version
                    echo "Deploying into NETLIFY_SITE_ID = ${NETLIFY_SITE_ID}"
                    node_modules/.bin/netlify status
                    node_modules/.bin/netlify deploy --dir=build --prod

                '''
            }
        }

        stage('Prod E2E'){
            agent{
                docker {
                    image 'mcr.microsoft.com/playwright:v1.51.1-noble'
                    reuseNode true
                }
            }
            environment{
                CI_ENVIRONMENT_URL='https://incomparable-starburst-8ff82a.netlify.app'
            }
            steps{
                echo "E2E"
                sh '''
                    npx playwright install
                    npx playwright test --reporter=html 
                '''
            }
            post{
                always {
                    publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, icon: '', keepAll: false, reportDir: 'playwright-report', reportFiles: 'index.html', reportName: 'PlayRight Prod Report', reportTitles: '', useWrapperFileDirectly: true])
                }
            }
        }      
    }
}
