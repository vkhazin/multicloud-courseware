# Lab: Native PaaS Monitoring

## Azure

1. There is a out-of -the-box monitoring avaialble which provides following information
2.  ```
    # Http Server Errors
    # Data In
    # Data Out
    # Request
    # Average Response Time
    ```
3. Microsoft also provide a monitoring tool named as Azure Application Insights - AAI
4. Open the PaaS resource ``` ex: App Service ``` and go to the Monitoring Section
4.1. Alerts section allows us to create new Alerts ``` ex: Storage Limitation, CPU Utilization, etc.``` 
4.2. Go to Metrics section under Monitoring section and check data based upon multiple ready-to-use metrics
4.3. Add Diagnostic Settings to capture following information 
4.4.    ```  
        # AppServiceHTTPLogs  
        # AppServiceConsoleLogs  
        # AppServiceAppLogs  
        # AppServiceFileAuditLogs  
        # AppServiceAuditLogs  
        # AllMetrics  
        ```
4.5 Go to Log Streem section and enble logs to monitor ``` ex: Applocation Logs, Web Logs ```
5. AAI is not free of cost and not enabled be default on resources such as App Service
6. To Enable AAI Go to the resource, ``` ex: App Service ``` and Overview -> Application Insights Or Go to the Settings under App Service and enable Application Insights
7. Azure Application Insghts [Pricing](https://azure.microsoft.com/en-us/pricing/details/monitor/)
