pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
            checkout scm
            }
        }
        
        stage('Build') 
            {
                agent{
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

                
            
        
        stage('Test')
        {
             agent{
                    docker{
                        image 'node:18-alpine'
                        reuseNode true
                    }
                }
             steps{
                sh '''
                if test -f build/index.html; then
                  echo "File exists"
                else
                  echo "File doesn't exist"
                exit 1
                fi

                npm test
                '''
             }

        }

        stage('E2E') {
          agent {
        docker {
            image 'mcr.microsoft.com/playwright:v1.39.0-focal'
            reuseNode true
        }
    }
    steps {
       
        sh '''
        npm install  serve
        ls -la build 
        node_modules/serve -s build &
        sleep 10
        npx playwright test --timeout
        '''
    }
    }

    }
    post {
         always {
            junit 'jest-results/junit.xml'
        }
    }    
}   






