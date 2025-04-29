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

        stage('E2E') {
    agent {
        docker {
            image 'mcr.microsoft.com/playwright:v1.39.0-focal'
'
            reuseNode true
        }
    }
    steps {
       
        sh '''
        echo "🌐 Installing serve..."
        npm install -g serve

        echo "📁 Checking if build directory exists:"
        ls -la build || { echo "❌ build folder is missing!"; exit 1; }

        echo "🚀 Starting local server..."
        npx serve -s build > serve.log 2>&1 &
        SERVER_PID=$!

        echo "🕒 Waiting 10 seconds for server to boot..."
        sleep 10

        echo "📜 Server logs:"
        cat serve.log

        echo "🧪 Running Playwright tests..."
        npx playwright test
        TEST_EXIT_CODE=$?

        echo "🛑 Killing local server..."
        kill $SERVER_PID || echo "⚠️ Failed to kill server"

        echo "✅ Finished with exit code $TEST_EXIT_CODE"
        exit $TEST_EXIT_CODE
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



