# Deploying a MEAN Application Using Azure Services

## Project Overview
This project demonstrates the deployment of a MEAN (MongoDB, Express.js, Angular, Node.js) application using various Azure services. Additionally, it includes the creation of a monitoring dashboard to visualize and analyze application statistics effectively.

---

## Objectives
1. Deploy a fully functional MEAN application using Azure services.
2. Ensure scalability and reliability by leveraging Azure's cloud infrastructure.
3. Create a monitoring dashboard to provide insights into application performance and usage statistics.

---

## Prerequisites
1. An active Azure account.
2. A functional MEAN application ready for deployment.
3. Basic understanding of Azure services.
4. Azure CLI installed and configured on your local machine.
5. Familiarity with monitoring tools such as Azure Monitor and Application Insights.

---

## Deployment Steps

### 1. Set Up Azure Resources
- **Resource Group**: Create a resource group to organize your resources.
  ```bash
  az group create --name <ResourceGroupName> --location <Region>
  ```
- **Virtual Machines**: Provision virtual machines if necessary for the backend or database services.
  ```bash
  az vm create --resource-group <ResourceGroupName> --name <VMName> --image UbuntuLTS --admin-username <Username> --generate-ssh-keys
  ```
- **Azure App Service**: Deploy the Node.js and Angular applications using Azure App Service.
  ```bash
  az appservice plan create --name <AppServicePlanName> --resource-group <ResourceGroupName> --sku F1
  az webapp create --name <WebAppName> --resource-group <ResourceGroupName> --plan <AppServicePlanName>
  ```
- **Azure Cosmos DB**: Set up a Cosmos DB instance with MongoDB API to store application data.
  ```bash
  az cosmosdb create --name <CosmosDBName> --resource-group <ResourceGroupName> --kind MongoDB
  ```

### 2. Deploy the MEAN Application
- **Backend (Node.js)**: 
  - Package the backend using npm.
    ```bash
    cd backend
    npm install
    ```
  - Deploy it to Azure App Service using `az webapp up` or CI/CD pipelines.
    ```bash
    az webapp up --name <WebAppName> --resource-group <ResourceGroupName> --runtime "NODE|14-lts"
    ```
- **Frontend (Angular)**: 
  - Build the Angular application using `ng build`.
    ```bash
    cd frontend
    ng build --prod
    ```
  - Deploy the static files to Azure Blob Storage or App Service.
    ```bash
    az storage account create --name <StorageAccountName> --resource-group <ResourceGroupName> --location <Region> --sku Standard_LRS
    az storage blob service-properties update --account-name <StorageAccountName> --static-website --index-document index.html
    az storage blob upload-batch -d '$web' --account-name <StorageAccountName> -s ./dist/<ProjectName>
    ```
- **Database (MongoDB)**:
  - Use Azure Cosmos DB to host the MongoDB database.
  - Configure connection strings in the application.
    ```bash
    CONNECTION_STRING=$(az cosmosdb list-connection-strings --name <CosmosDBName> --resource-group <ResourceGroupName> --query connectionStrings[0].connectionString -o tsv)
    ```

### 3. Configure Networking and Security
- Enable Application Gateway or Azure Front Door for traffic management.
  ```bash
  az network application-gateway create --name <GatewayName> --resource-group <ResourceGroupName> --sku Standard_v2 --location <Region> --public-ip-address <PublicIPName>
  ```
- Configure Network Security Groups (NSGs) for secure communication.
  ```bash
  az network nsg create --name <NSGName> --resource-group <ResourceGroupName>
  ```
- Use Managed Identity to securely access Azure resources.
  ```bash
  az webapp identity assign --name <WebAppName> --resource-group <ResourceGroupName>
  ```

### 4. Set Up Monitoring Dashboard
- Enable **Azure Monitor** and **Application Insights** for tracking application metrics.
  ```bash
  az monitor log-analytics workspace create --resource-group <ResourceGroupName> --workspace-name <WorkspaceName>
  az monitor app-insights component create --app <AppName> --location <Region> --resource-group <ResourceGroupName>
  ```
- Create custom dashboards in the Azure portal to display:
  - CPU and memory usage.
  - API response times.
  - Database query performance.
  - Error and exception tracking.
- Integrate Azure Log Analytics for advanced querying.
  ```bash
  az monitor log-analytics query --workspace <WorkspaceName> --analytics-query "<KQL Query>"
  ```

---

## Reference Link
For detailed guidance and additional tips, refer to the following repository:
[ResumeAI GitHub Repository](https://github.com/UnpredictablePrashant/ResumeAI)

---

## Submission Instructions
1. Ensure that the MEAN application is fully deployed and accessible via a public endpoint.
2. Provide screenshots of the deployed application and the monitoring dashboard.
3. Push all relevant code and documentation to a GitHub repository.
4. Share the repository link for submission.

---

## Conclusion
By following the above steps, you will have successfully deployed a MEAN application using Azure services while ensuring effective monitoring and performance tracking. This setup not only showcases your technical skills but also demonstrates the ability to leverage cloud infrastructure effectively.

