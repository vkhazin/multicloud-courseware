# Lab: Native PaaS Monitoring

## AWS

1. [AWS CloudTrail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-create-and-update-a-trail.html) - It is enabled by default with aws account creation
2. Provides following information: ``` Record of actions taken by a user, role, or an AWS service in Elastic Beanstalk
3. Creating a Trail from events history captured by CloudTrail, an deliver logs data to S3 Buckets
4. [AWS CloudWatch](https://aws.amazon.com/cloudwatch/) provides logs at a large scope and provide information regarding  applicaton insights and enables to collect, view, analuze, and view system metrics 
5. CloudWatch enables automatically with the provisioning of some PaaS resource such as Azure Beansstalk
6. You can navigate to the [Amazon CloudWatch](https://aws.amazon.com/cloudwatch/) console to see your dashboard and get an overview of all of your resources as well as your alarms 
7. Allows us to set alarms based on resource performace, error logs, etc. 
8. We can also publish our own metrics directly to Amazon CloudWatch   
9. CloudWatch [Pricing](https://aws.amazon.com/cloudwatch/pricing/)

## Azure

1. There is a out-of -the-box monitoring avaialble which provides following information
2.  - Http Server Errors
    - Data In
    - Data Out
    - Request
    - Average Response Time  
3. [Azure Application Insights](https://azure.microsoft.com/en-us/services/monitor/) - AAI - Paid Monitoring tool  
4. Open the PaaS resource ``` ex: App Service ``` and go to the Monitoring Section  
5. Alerts section allows us to create new Alerts ``` ex: Storage Limitation, CPU Utilization, etc.```  
6. Go to Metrics section under Monitoring section and check data based upon multiple ready-to-use metrics  
7. Add Diagnostic Settings to capture following information  
8.  - AppServiceHTTPLogs
    - AppServiceConsoleLogs
    - AppServiceAppLogs
    - AppServiceFileAuditLogs
    - AppServiceAuditLogs
    - AllMetrics  
9. Go to Log Streem section and enble logs to monitor ``` ex: Applocation Logs, Web Logs ```  
10. AAI is not free of cost and not enabled be default on resources such as App Service  
11. To Enable AAI Go to the resource, ``` ex: App Service ``` and Overview -> Application Insights Or Go to the Settings under App Service and enable Application Insights  
12. Azure Application Insghts [Pricing](https://azure.microsoft.com/en-us/pricing/details/monitor/)

## GCP

1. For Basic Metric: At Project Page, go to Instances to check logs regarding CPU Performance, Memory usage, and Network Traffic at a higher level such as Black-Box Monitoring
2. There is no such extensive native monitoring in GCP though it provides stackdriver integration which gives the feel for native monitoring not particularly for Google App Engine
3. Here is [Quick Guide](https://cloud.google.com/monitoring/docs/how-to) to setup monitoring for some GCP services such as GCE.
4. StackDriver [Pricing](https://cloud.google.com/stackdriver/pricing)
5. There are other tools available to get more enhanced view of logs, events such as [DataDog](https://docs.datadoghq.com/integrations/google_cloud_platform/), [DynaTrace](https://www.dynatrace.com/technologies/google-cloud-platform-monitoring/), etc.
