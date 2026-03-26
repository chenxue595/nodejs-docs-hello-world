pipeline {
    agent any
    environment {
        AZURE_SUBSCRIPTION_ID = '624865c7-482b-4afe-9fee-ff810fc86bcd'
        AZURE_TENANT_ID = 'b6fa8a4d-a48e-4fc1-8493-59ba871d4d77'
        RESOURCE_GROUP = 'jenkins-get-started-rg'
        WEBAPP_NAME = 'jenkins-node-app-chen'
    }
    stages {
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Azure Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'AzureServicePrincipal',
                    usernameVariable: 'AZURE_CLIENT_ID',
                    passwordVariable: 'AZURE_CLIENT_SECRET'
                )]) {
                    sh '''
                    az login --service-principal \
                    --username $AZURE_CLIENT_ID \
                    --password $AZURE_CLIENT_SECRET \
                    --tenant $AZURE_TENANT_ID
                    '''
                }
            }
        }
        stage('Deploy to Azure') {
            steps {
                sh '''
                zip -r app.zip .
                az webapp deploy \
                --resource-group $RESOURCE_GROUP \
                --name $WEBAPP_NAME \
                --src-path app.zip \
                --type zip
                '''
            }
        }
    }
}
