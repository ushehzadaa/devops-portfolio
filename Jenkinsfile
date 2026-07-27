pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Source code retrieved from GitHub successfully.'
                checkout scm
            }
        }

        stage('Verify Repository') {
            steps {
                echo 'Displaying repository files...'
                sh 'ls -la'
            }
        }

        stage('Validation') {
            steps {
                echo 'Validating the DevOps portfolio repository...'
                sh 'test -f README.md'
            }
        }
    }

    post {
        success {
            echo 'CI pipeline completed successfully.'
        }

        failure {
            echo 'CI pipeline failed. Review the console output.'
        }
    }
}
