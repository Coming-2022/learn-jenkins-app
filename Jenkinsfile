pipeline {
    agent any

    stages {
        /* 
        This is a comment.
        It is using for jenkins build.
        */
        stage('Preparation') {
            agent{
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                echo 'Preparation Steps'
                sh '''
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm test
                '''
            }
        }
        stage('Build') {
            agent{
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                echo 'Start Steps'
                sh '''
                    ls -la
                    npm run build
                '''
            }
        }
        
        stage ('Run Tests') {
            parallel {
                stage('Test') {
                    agent{
                        docker {
                            image 'node:18-alpine'
                            reuseNode true
                        }
                    }
                    steps {
                        echo 'Test Steps'
                        sh '''
                            test -f build/index.html
                        '''
                    }
                }
                
                stage('E2E') {
                    steps {
                        echo 'E2E Steps'
                        sh '''
                            test -f build/index.html
                        '''
                    }
                }
                
            }
        }
        
        
    }
    post {
        always {
            junit 'test-results/junit.xml'
            publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'playwright-report', reportFiles: 'index.html', reportName: 'Playwright HTML Report', reportTitles: '', useWrapperFileDirectly: true])
        }
    }
}