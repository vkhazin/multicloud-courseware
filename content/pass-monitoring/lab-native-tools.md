# Lab: Native Monitoring Tools

## Aws

1. Navigate to Elastic Beanstalk using [https://console.aws.amazon.com](https://console.aws.amazon.com)
2. Select node.js application we have deployed in the previous lab
3. From the Elastic Beanstalk application's page find links to health, monitoring, alarms, and logs
4. Health will display the overall status of the environment including the status of each host and response status codes
5. Monitoring will share familiar utilization metrics
6. Alarms allow automation on metrics, such as too many 500 error codes - send a notification
7. And Logs allow us to download the logs and find `Server is running` log item our node.js container is writing when started
8. CloudWatch allows us to add Elastic Beanstalk metrics to a dashboard as well

## Azure

1. Navigate to App Service using [https://portal.azure.com](https://portal.azure.com)
2. Select the node.js application we have deployed in the previous lab
3. Locate Activity Log, Metrics and Log Stream
4. Activity log should provide a "paper" trail for administrative actions
5. Metrics will display familiar options resource utilization information, e.g. `CPU Time`
6. And Log Stream will display a semi-live log with a delay up to 5 min, a bit of a pet peeve
7. There is also Azure Monitoring you can use to add App Service metrics to add to a dashboard

## GCP

1. Navigate to App Engine using web console [https://console.cloud.google.com/](https://console.cloud.google.com/)
2. GCP Supports one App Engine per project with, possibly, [multiple services](https://cloud.google.com/appengine/docs/standard/nodejs/an-overview-of-app-engine)
3. Select Instances to get to metrics
4. Metrics are not instant, hopefully, you will get some metrics as a result of the previous lab setting up App Engine
5. There is a drop-down for `Version` , should have as many as we have deployed, and there is a drop-down list for metrics, the default value will be `Summary` more utilization details are available when we select the drop-down list
6. App Engine is also integrated with the [Stackdriver](https://cloud.google.com/appengine/docs/standard/nodejs/an-overview-of-app-engine)



