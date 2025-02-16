pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker{
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

    stage('test'){
        agent {
                docker{
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
    }
    stage('e2e') {
            agent {
                docker{
                    image 'docker pull mcr.microsoft.com/playwright:v1.50.1-noble'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm instal -g serve
                    serve -s build
                    npx playwright tests
                '''
            }
        }
    }
    post{
        always{
            junit 'test-results/junit.xml'
        }
    }
}
