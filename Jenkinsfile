pipeline {
    agent any
    tools {
        go 'go-1.19' // Comes from the jenkins global config
    }
    environment {
        ENV = "${env.BRANCH_NAME == 'master' ? 'PROD' : 'DEV'}" // Define the ENV based on the branch name
        BRANCH = "${env.BRANCH_NAME}"
    }
    options {
        skipDefaultCheckout(true)
    }

    stages {
        
        stage('Send initial notification') {
            when {
                anyOf {
                    branch 'master';
                    branch 'develop'
                }
            }
            steps {
                sh '''
                    echo "Current commit:"
                    git rev-parse HEAD

                    echo "README line count:"
                    wc -l README.md

                    echo "README contents:"
                    cat -n README.md

                    echo "Git status:"
                    git status --short

                    echo "Tracked README from current commit:"
                    git show HEAD:README.md
                '''
                slackSend channel: '#all-udemyjenkinslab',
                          message: "Build for job ${env.JOB_NAME} has started - (<${env.BUILD_URL}|Open>)"
            }
        }
        stage('Clean Checkout') {
            steps {
                deleteDir()
                checkout scm
            }
        }

        stage('Scan for secrets') {
            steps {
                sh '''
                    curl -LO https://github.com/zricethezav/gitleaks/releases/download/v8.9.0/gitleaks_8.9.0_linux_x64.tar.gz
                    tar -xzf gitleaks_8.9.0_linux_x64.tar.gz
                    ./gitleaks protect -v // Scan for commonly leaked secrets
                    rm -rf gitleaks*
                '''
            }
        }
        stage('Build') {
            steps {
                sh 'bash scripts/build.sh' // Run the build
            }
        }
        stage('Test') {
            steps {
                sh 'bash scripts/test.sh' // Run the test
            }
        }
        stage('Deploy') {
            when {
                anyOf {
                    branch 'master';
                    branch 'develop'
                }
            }
            steps {
                sh 'export JENKINS_NODE_COOKIE=do_not_kill ; bash scripts/deploy.sh'
            }
        }
    }
    post {
        always {
            slackSend channel: '#all-udemyjenkinslab',
                      color: "${currentBuild.currentResult == 'SUCCESS' ? 'good' : 'danger'}",
                      message: "Build for job ${env.JOB_NAME} finished with status ${currentBuild.currentResult} - (<${env.BUILD_URL}|Open>)"
        }
    }
}

