pipeline {
    agent { label 'windows' }
    

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
