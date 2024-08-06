pipeline {
    agent any

    environment {

        SQL_PACKAGE_PATH = '"C:\\Users\\Administrator\\.dotnet\\tools\\sqlpackage.exe"' // Adjust the path as needed
        SERVER_NAME = 'WIN-C0L67N6I1KF\\SQLEXPRESS'
        USERNAME = 'sqladmin'
        PASSWORD = credentials('sqlserver-db-password') // Store sensitive data like this in Jenkins credentials for security
        DATABASE_NAME = 'employeedb'
        AZ_CLIENT_SECRET = '"C:\\temp\\backup\\backup.bacpac"' // Adjust the path as needed

        // Azure credentials details 
        AZ_CLIENT_ID = credentials('az-appreg-client-id')
        AZ_CLIENT_SECRET= credentials('az-appreg-client-secret')
        AZ_TENANT_ID = credentials('az-appreg-tenant-id')
        AZURE_SUBSCRIPTION_ID =credentials('az-subscription-id')
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
        stage('CONNECT azure ') {
            steps {
                script {
              
                    // Login to az sub
                    def command1 = " az login --service-principal -u ${env.AZ_CLIENT_ID} -p ${env.AZ_CLIENT_SECRET} --tenant ${env.AZ_TENANT_ID}"
                    

                    // Print the command (optional, for debugging purposes)
                    echo "Running command: ${command1}"
                    
                    // Execute the command
                    try {
                        // Run the batch command and capture the return status
                        def returnStatus1 = bat(script: command1, returnStatus: true)
                                           
                     
                        // Check the status and handle errors
                        if (returnStatus1 != 0) {
                            error "Command1 failed with exit status ${returnStatus1}"
                        } else {
                            echo "Command1 succeeded1"
                        }
                    } catch (Exception e) {
                        // Handle any unexpected errors here
                        echo "An error1 occurred: ${e.message}"
                        currentBuild.result = 'FAILURE'
                    }


                // sh  '''
                //     az account set --subscription ${env.AZURE_SUBSCRIPTION_ID}
                //     '''
                    

                    // Creating data base in azure
                    //  def command2 = " az sql db create --resource-group rg-devops-demo --server targetsqlserver --name employeedb --service-objective S0 --backup-storage-redundancy Local"
                    
             
                    // echo "Running command: ${command2}"
                    
                  
                    // try {
                  
                    //   def returnStatus2 = bat(script: command2, returnStatus: true)
                                           
                     
                       
                    //     if (returnStatus2 != 0) {
                    //         error "Command2 failed with exit status ${returnStatus2}"
                    //     } else {
                    //         echo "Command2 succeeded2"
                    //     }
                    //  } catch (Exception e) {
                  
                    //     echo "An error2 occurred: ${e.message}"
                    //    currentBuild.result = 'FAILURE'
                     }


                // importing bacpac file to  azure target machine

                    def command3 = "az sql db import --resource-group ${env.RESOURCE_GROUP} --server ${env.SQL_SERVER} --name ${env.SQL_DATABASE} --admin-user ${env.AZ_CLIENT_ID} --admin-password ${env.AZ_CLIENT_SECRET} --bacpac-file ${env.AZ_CLIENT_SECRET}"                    
                    // Print the command (optional, for debugging purposes)
                    echo "Running command: ${command3}"
                    
                    // Execute the command
                    try {
                        // Run the batch command and capture the return status
                        def returnStatus = bat(script: command3, returnStatus: true)
                                           
                     
                        // Check the status and handle errors
                        if (returnStatus != 0) {
                            error "Command3 failed with exit status ${returnStatus}"
                        } else {
                            echo "Command3 succeeded"
                        }
                    } catch (Exception e) {
                        // Handle any unexpected errors here
                        echo "An error occurred: ${e.message}"
                        currentBuild.result = 'FAILURE'

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
