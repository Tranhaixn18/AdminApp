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
                            # Kiểm tra curl, nếu không có thì báo lỗi
                            command -v curl >/dev/null 2>&1 || { echo "Curl không có, cần cài trước hoặc dùng image có curl"; exit 1; }
                            # Tải script cài đặt .NET
                            curl -sSL https://dot.net/v1/dotnet-install.sh -o dotnet-install.sh
                            chmod +x dotnet-install.sh
                            # Cài phiên bản .NET 5 mới hơn (5.0.408)
                            ./dotnet-install.sh --version 5.0.408
                            # Thêm dotnet vào PATH
                            export PATH="$PATH:/var/jenkins_home/.dotnet"
                            # Kiểm tra cài đặt
                            /var/jenkins_home/.dotnet/dotnet --version
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