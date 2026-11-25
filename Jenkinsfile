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
                sh 'bash scripts/build.sh' // Run the build.sh asset
            }
        }
        stage('Test') {
            steps {
                sh 'bash scripts/test.sh' // Run the test.sh asset
            }
        }
<<<<<<< HEAD
        stage('Deploy'){
            steps {
                sh 'bash scripts/deploy.sh' // Run the deploy.sh asset
            }
        }
=======
>>>>>>> 25bd6852046df3e4f301b1c4f23494242eec0839
    }
}
