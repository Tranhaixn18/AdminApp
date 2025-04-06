pipeline{
    agent: any
    stages{
        stage('Setup .NET 5') {
            steps {
                script {
                    def dotnetInstalled = sh(script: 'dotnet --version', returnStatus: true) == 0
                    if (!dotnetInstalled) {
                        echo ".NET 5 chưa được cài đặt, đang tiến hành cài đặt..."
                        sh '''
                            wget https://packages.microsoft.com/config/debian/11/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
                            dpkg -i packages-microsoft-prod.deb
                            apt-get update
                            apt-get install -y dotnet-sdk-5.0
                            ln -s /usr/lib/dotnet/dotnet /usr/bin/dotnet
                        '''
                    } else {
                        echo ".NET 5 đã sẵn sàng!"
                    }
                }
            }
        }

        stage('Build Project'){
            steps{
                sh ''''
                dotnet build AdminApp.sln
                '''
            }
        }
    }
}