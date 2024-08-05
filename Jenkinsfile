pipeline {
    agent any

    environment {
        SQL_PACKAGE_PATH = '"C:\\Program Files\\Microsoft SQL Server\\MSSQL14.SQLEXPRESS\\MSSQL\\Bin\\sqlservr.exe"' // Adjust the path as needed
        SERVER_NAME = 'WIN-C0L67N6I1KF\\SQLEXPRESS'
        USERNAME = 'sqladmin'
        PASSWORD = 'jenkins@1234' // Store sensitive data like this in Jenkins credentials for security
        DATABASE_NAME = 'employeedb'
        BACAPC_FILE = '"C:\\temp\\backup\\backup.bacpac"' // Adjust the path as needed
    }

    stages {
        stage('Export Database') {
            steps {
                script {
                    // Construct the sqlpackage command
                    def command = "${env.SQL_PACKAGE_PATH} /Action:Export /SourceServerName:${env.SERVER_NAME} /SourceDatabaseName:${env.DATABASE_NAME} /SourceUser:${env.USERNAME} /SourcePassword:${env.PASSWORD} /TargetFile:${env.BACAPC_FILE}"
                    
                    // Print the command (optional, for debugging purposes)
                    echo "Running command: ${command}"
                    
                    // Execute the command
                    bat(script: command, returnStatus: true)
                }
            }
        }
    }
    
    post {
        always {
            // Archive the .bacpac file
            archiveArtifacts artifacts: '**/*.bacpac', allowEmptyArchive: true
        }
    }
}
