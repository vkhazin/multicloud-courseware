# Lab: Native Serverless Monitoring

## AWS

1. AWS Lambda monitors functions and sends metrics data to Amazon CloudWatch
2. To access metrics using the CloudWatch console
3. Open the [CloudWatch console](https://console.aws.amazon.com/cloudwatch/)
4. In the navigation pane, choose **Metrics**
5. In the **CloudWatch Metrics by Category** pane, choose **Lambda Metrics**

## Azure

1. Azure Functions uses the Application Insights native tool of Azure to provide the Monitoring of the functions
2. For a function app to send data to Application Insights, it requires instrumentation key of an Application Insights resource. The key must be in an app setting named as: `APPINSIGHTS_INSTRUMENTATIONKEY`
3. Application Insights integration is enabled by default when a new function app is created with Instrument Key generated under Application Settings
4. When Function App is created via CLI, Visual Studio, or VS Code, it requires manually setting up Application Insights
5. To manually integrate application insight, go to the Azure Portal and fucntion to enable insights. It will show warning that insghits are not configured and link to configure it
6. Go to **Function app** and under **Overview Tab** click on **Application Insights** under Configured Features Section 
7. To monitor a specific function of function app **select any particular function** then go to **Monitor Tab**, from there it can be openend in Application Insights tool directly
8. Azure Insights [Pricing](https://docs.microsoft.com/en-us/azure/azure-monitor/app/pricing)
