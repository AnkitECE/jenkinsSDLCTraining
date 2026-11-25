pipeline {
    agent any

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
                echo 'Testing Go application'
            }
        }
    }
}
