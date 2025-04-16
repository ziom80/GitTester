pipeline {
    agent any

    

    triggers {
        cron('H/15 * * * *')  // This runs the pipeline every 15 minutes
    }


   
       
    

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

               stage('Install .NET Core SDK 2.1') {
                steps {
                sh '''
                    # Download dotnet-install.sh if wget is missing, try curl
                    if command -v curl >/dev/null 2>&1; then
                        curl -SL https://dotnet.microsoft.com/download/dotnet/scripts/v1/dotnet-install.sh -o dotnet-install.sh
                    elif command -v wget >/dev/null 2>&1; then
                        wget https://dotnet.microsoft.com/download/dotnet/scripts/v1/dotnet-install.sh -O dotnet-install.sh
                    else
                        echo "Neither curl nor wget is available. Install one of them first."
                        exit 1
                    fi
        
                    chmod +x dotnet-install.sh
        
                    # Install .NET Core SDK 2.1 into local user folder
                    ./dotnet-install.sh --version 2.1.818 --install-dir $HOME/.dotnet
        
                    # Add to PATH for this session
                    export PATH=$HOME/.dotnet:$PATH
        
                    # Verify installation
                    $HOME/.dotnet/dotnet --version
                '''
            }
        }


        
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
