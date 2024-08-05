pipeline {
    agent any

    environment {

        SQL_PACKAGE_PATH = '"C:\\Users\\Administrator\\.dotnet\\tools\\sqlpackage.exe"' // Adjust the path as needed
        SERVER_NAME = 'WIN-C0L67N6I1KF\\SQLEXPRESS'
        USERNAME = 'sqladmin'
        PASSWORD = 'jenkins@1234' // Store sensitive data like this in Jenkins credentials for security
        DATABASE_NAME = 'employeedb1'
        BACAPC_FILE = '@"C:\\temp\\backup\\backup.bacpac"' // Adjust the path as needed
    }

    stages {
        stage('Export Database bacpac to local machine') {
            steps {
                script {
              
                    // Construct the sqlpackage command
                    def command = " ${env.SQL_PACKAGE_PATH} /Action:Export /SourceServerName:${env.SERVER_NAME} /SourceDatabaseName:${env.DATABASE_NAME} /SourceUser:${env.USERNAME} /SourcePassword:${env.PASSWORD} /TargetFile:${env.BACAPC_FILE} /SourceTrustServerCertificate:true"
                    
                    // Print the command (optional, for debugging purposes)
                    echo "Running command: ${command}"
                    
                    // Execute the command
                    try {
                        // Run the batch command and capture the return status
                        def returnStatus = bat(script: command, returnStatus: true)
                                           
                     
                        // Check the status and handle errors
                        if (returnStatus != 0) {
                            error "Command failed with exit status ${returnStatus}"
                        } else {
                            echo "Command succeeded"
                        }
                    } catch (Exception e) {
                        // Handle any unexpected errors here
                        echo "An error occurred: ${e.message}"
                        currentBuild.result = 'FAILURE'
                    }
                }
            }
        }
    }
    
    // post {
    //     always {
    //         // Archive the .bacpac file
    //         archiveArtifacts artifacts: '**/*.bacpac', allowEmptyArchive: true
    //     }
    // }
}
