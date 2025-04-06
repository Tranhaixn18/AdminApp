pipeline{
    agent any
    stages{
        stage('Setup .NET 5') {
            steps {
                script {
                    // Kiểm tra xem dotnet có sẵn không
                    def dotnetInstalled = sh(script: 'dotnet --version', returnStatus: true) == 0
                    if (!dotnetInstalled) {
                        echo ".NET 5 chưa được cài đặt, đang tiến hành cài đặt..."
                        sh '''
                            # Cài curl nếu chưa có
                            apt-get update || true
                            apt-get install -y curl || true
                            # Tải và chạy script cài đặt .NET 5
                            curl -sSL https://dot.net/v1/dotnet-install.sh -o dotnet-install.sh
                            chmod +x dotnet-install.sh
                            ./dotnet-install.sh --version 5.0.17
                            # Thêm dotnet vào PATH cho phiên hiện tại
                            export PATH="$PATH:/var/jenkins_home/.dotnet"
                        '''
                        // Cập nhật PATH cho các bước sau
                        env.PATH = "${env.PATH}:/var/jenkins_home/.dotnet"
                    } else {
                        echo ".NET 5 đã được cài đặt, tiếp tục build..."
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