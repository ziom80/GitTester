pipeline {
    agent any

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

        stage('Build') {
            steps {
                echo 'Building the application...'
               bat 'dotnet build GitTester.csproj --configuration Release --no-restore'
                // For example: compiling Java or building Docker images
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
