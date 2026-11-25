pipeline {
    agent any
    tools { go 'go-1.19' } 
    environment {
        ENV = "${env.BRANCH_NAME == 'master' ? 'PROD' : 'DEV'}"
    }

    stages {    
        stage('Checkout') {
            steps {
                echo 'Source code checked out'
            }
        }

        stage('Build') {
            steps {
                echo 'Building Go application'
            	sh 'go version'
                sh 'go build -o main main.go'
	        }
        }
        stage('Test') {
            steps {
                sh 'bash scripts/test.sh' // Run the test.sh asset
            }
        }
        stage('Deploy'){
            steps {
                sh '''
                    export JENKINS_NODE_COOKIE=do_not_kill
                    export BRANCH=$BRANCH_NAME
                    bash scripts/deploy.sh
                '''
            }
        }
    }
}
