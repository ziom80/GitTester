pipeline {
    agent any

    stages {
        stage('Check OS') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'uname -a'
                    } else {
                        bat 'ver'
                    }
                }
            }
        }
    }

    triggers {
        cron('H/15 * * * *')  // This runs the pipeline every 15 minutes
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Cloning repository..'
                checkout scm
            }
        }

        stage('Restore Dependencies') {
            steps {
                script {
                    sh 'dotnet restore GitTester.csproj'
                }
            }
        }

        stage('Build Application') {
            steps {
                script {
                    sh 'dotnet build GitTester.csproj --configuration Release --no-restore'
                }
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                // Example: sh 'npm test' or ./gradlew test
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application...'
                // Example: sh './deploy.sh'
            }
        }
    }

    post {
        success {
            echo 'Build completed successfully!'
        }
        failure {
            echo 'Build failed. Please check the logs.'
        }
    }
}
