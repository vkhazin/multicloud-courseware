# Lab: Native Serverless Monitoring

## AWS

1. AWS Lambda monitors functions and sends metrics data to Amazon CloudWatch
2. Metrics and logs are accessible from the console under Lambda service and in CloudWatch
3. To access metrics using the CloudWatch console, navigate the [CloudWatch console](https://console.aws.amazon.com/cloudwatch/)
4. In the navigation pane, choose `Metrics`
5. In the CloudWatch Metrics by Category choose Lambda Metrics
6. Explore the options and metrics available

## Azure

1. Azure Functions uses the Application Insights native tool to provide the Monitoring of the functions
2. For a function app to send data to Application Insights, it requires an instrumentation key of an Application Insights resource. The key must be in an app setting named `APPINSIGHTS_INSTRUMENTATIONKEY`
3. Application Insights integration is enabled by default when a new function app is created with Instrument Key generated under Application Settings
4. When Function App is created via CLI, Visual Studio, or VS Code, it requires manually setting up Application Insights
5. To manually integrate application insight, go to the Azure Portal and function to enable insights.
6. It will show a warning that insights are not configured and link to configure
7. Proceed to `Function App`, under `Overview Tab`, select `Application Insights`to access the metrics
8. Can also monitor a specific function from the `Monitor` tab of the Function from the list of Function Apps

## GCP

1. Cloud Functions includes simple logging by default
2. Logs written to stdout or stderr will appear automatically in the [Cloud Console](https://console.cloud.google.com/project/_/logs?service=cloudfunctions.googleapis.com&_ga=2.216174609.-665913479.1577620601)
3. Logs for Cloud Functions are viewable in the Stackdriver Logging UI, and via the gcloud command-line tool
4. To view logs with the `gcloud` cli from Google Cloud Shell, use the following command:
5. `gcloud functions logs read`
6. Can also view logs for Cloud Functions from the [Cloud Console](https://console.cloud.google.com/project/_/logs?service=cloudfunctions.googleapis.com&_ga=2.186241966.-665913479.1577620601) similarily we have seen for the PaaS



