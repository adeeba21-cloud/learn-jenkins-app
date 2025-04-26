pipeline {
    agent any

    stages {
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
         post {
        always {
            junit 'test-results/junit.xml'
        }
         }
    }

    
}



