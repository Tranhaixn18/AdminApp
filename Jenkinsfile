pipeline {
    agent any
    stages {
        stage('Setup .NET 5') {
            steps {
                script {
                    def dotnetInstalled = sh(script: 'dotnet --version', returnStatus: true) == 0
                    if (!dotnetInstalled) {
                        echo ".NET 5 chưa được cài đặt, đang tiến hành cài đặt..."
                        sh '''
                            # Kiểm tra curl
                            command -v curl >/dev/null 2>&1 || { echo "Curl không có, cần cài trước hoặc dùng image có curl"; exit 1; }
                            # Tải .NET 5 SDK đầy đủ
                            curl -sSL https://download.visualstudio.microsoft.com/download/pr/8e3f996e-3e88-4a3e-9e5d-b537dff48a4e/5e9e6c46247cf8d61439616fcdb7af5f/dotnet-sdk-5.0.408-linux-x64.tar.gz -o dotnet-sdk.tar.gz
                            mkdir -p /var/jenkins_home/dotnet
                            tar -xzf dotnet-sdk.tar.gz -C /var/jenkins_home/dotnet
                            # Thêm dotnet vào PATH
                            export PATH="$PATH:/var/jenkins_home/dotnet"
                            # Bật chế độ Invariant để không cần ICU
                            export DOTNET_SYSTEM_GLOBALIZATION_INVARIANT=1
                            # Kiểm tra cài đặt
                            /var/jenkins_home/dotnet/dotnet --version
                        '''
                        env.PATH = "${env.PATH}:/var/jenkins_home/dotnet"
                        env.DOTNET_SYSTEM_GLOBALIZATION_INVARIANT = "1"
                    } else {
                        echo ".NET 5 đã được cài đặt, tiếp tục build..."
                        env.DOTNET_SYSTEM_GLOBALIZATION_INVARIANT = "1"
                    }
                }
            }
        }
        stage('Build Project') {
            steps {
                sh '''
                    dotnet build AdminApp.sln
                '''
            }
        }
    }
}