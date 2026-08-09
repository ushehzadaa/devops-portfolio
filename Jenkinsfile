pipeline {
    agent any

    environment {
        IMG = "smartavtool"
        APP = "smartavtool-app"
    }

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
                echo 'Validating the SmartAV Tool repository...'
                sh '''
                    test -f README.md
                    test -f Dockerfile
                    test -f app/index.html
                '''
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    env.TAG = sh(script: 'git rev-parse --short HEAD', returnStdout: true).trim()
                }
                echo "Building Docker image ${IMG}:${TAG}"
                sh 'docker build -t ${IMG}:${TAG} .'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying the latest SmartAV Tool container...'
                sh 'docker rm -f ${APP} 2>/dev/null || true'
                sh 'docker run -d --name ${APP} -p 80:80 --restart unless-stopped ${IMG}:${TAG}'
            }
        }

        stage('Deployment Verification') {
            steps {
                echo 'Verifying deployed application...'
                sh 'sleep 10'
                sh 'docker ps --filter "name=${APP}"'
                sh 'curl -fsS 127.0.0.1 > /dev/null'
                sh 'docker inspect ${APP} --format="Container={{.Name}} Status={{.State.Status}}"'
            }
        }
    }

    post {
        success {
            echo 'CI/CD pipeline completed successfully.'
        }

        failure {
            echo 'CI/CD pipeline failed. Review the console output.'
        }
    }
}
