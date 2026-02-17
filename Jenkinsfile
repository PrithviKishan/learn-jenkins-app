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
                export npm_config_cache=$PWD/.npm
                ls -la
                node --version
                npm --version
                npm ci
                npm run build
                ls -la
                '''
            }
        }
       
        stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }          
            steps {
                sh  '''
                test -f build/index.html
                npm test
                '''
            }
        }
        stage('e2e') {
            agent {
                docker {
                    image 'docker pull mcr.microsoft.com/playwright:v1.58.2-noble'
                    reuseNode true
                }
            }          
            steps {
                sh  '''
                npm install -g serve
                serve -s build
                npx playwright test
                '''
            }
        }        
    }
    post
    {
        always{
            junit 'test-results/junit.xml'
        }
    }
}