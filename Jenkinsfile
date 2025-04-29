pipeline {
    agent any

    stages {
        /*
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
                
            
        }*/
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

        stage('E2E')
        {
             agent{
                    docker{
                        image 'mcr.microsoft.com/playwright:v1.52.0-noble'
                        reuseNode true
                    }
                }
             steps{
                sh '''
                npm install -g serve
                node_modules/serve-index -s build &
               SERVER_PID=$!
               sleep 10
                npx playwright test
                TEST_EXIT_CODE=$?
                kill $SERVER_PID
                exit $EXIT_CODE

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



