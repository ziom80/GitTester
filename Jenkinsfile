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

        


        
        stage('Checkout') {
            steps {
                echo 'Cloning repository..'
                checkout scm
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

                     export DOTNET_SYSTEM_GLOBALIZATION_INVARIANT=true
                     export PATH=$HOME/.dotnet:$PATH
        
                    # Verify installation
                    $HOME/.dotnet/dotnet --version
                '''
            }
        }

        stage('Install OpenSSL') {
            steps {
                sh '''
                    apt-get update
                    apt-get install -y libssl1.1 || apt-get install -y libssl1.0.0 || apt-get install -y libssl-dev
                '''
            }
        }

        

       stage('Build .NET Console App') {
                steps {
                    sh '''
                        export PATH=$HOME/.dotnet:$PATH
                        export DOTNET_SYSTEM_GLOBALIZATION_INVARIANT=true
                        export DOTNET_SYSTEM_NET_HTTP_USESOCKETSHTTPHANDLER=0
                        dotnet --version
                        dotnet restore GitTester.csproj
                        dotnet build GitTester.csproj -c Release
                    '''
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
