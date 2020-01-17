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

## GCP

1. Cloud Functions includes simple logging by default
2. Logs written to stdout or stderr will appear automatically in the [Cloud Console](https://console.cloud.google.com/project/_/logs?service=cloudfunctions.googleapis.com&_ga=2.216174609.-665913479.1577620601)
3. Write Logs for `Node.js` Functions: 
4.  ```
    exports.helloWorld = (req, res) => {
      console.log('I am a log entry!');
      console.error('I am an error!');
      res.end();
    };
    ```
5. Write Logs for `Python` Functions: 
6.  ```
    def hello_world(data, context):
      """Background Cloud Function.
      Args:
           data (dict): The dictionary with data specific to the given event.
           context (google.cloud.functions.Context): The event metadata.
      """
      print('Hello, stdout!')
    ```
7. Write Logs for `GO` Functions: 
8.  ```
    // Package helloworld provides a set of Cloud Functions samples.
    package helloworld

    import (
            "fmt"
            "log"
            "net/http"
    )

    // HelloLogging logs messages.
    func HelloLogging(w http.ResponseWriter, r *http.Request) {
            log.Println("This is stderr")
            fmt.Println("This is stdout")
    }
    ```
9. **Note**:  
    * Logs emitted using `console.log()` have the `INFO` log level.
    * Logs emitted using `console.error()` have the `ERROR` log level.
    * Logs written directly to `stdout` or `stderr` do not have an associated log level.
    * Internal system messages have the `DEBUG` log level.
10. **Viewing Logs**:
11. Using the command-line tool
12. Logs for Cloud Functions are viewable in the Stackdriver Logging UI, and via the gcloud command-line tool
13. To view logs with the gcloud tool, use the '[logs read](https://cloud.google.com/sdk/gcloud/reference/functions/logs/read)' command:
14. `gcloud functions logs read`
15. To view the logs for a specific function, provide the function name as an argument:
16. `gcloud functions logs read FUNCTION_NAME`
17. You can even view the logs for a specific execution:
18. `gcloud functions logs read FUNCTION_NAME --execution-id EXECUTION_ID`
19. For the full range of log viewing options, view the help for logs read:
20. gcloud functions logs read -h
21. Using the Loggin Dashboard: You can also view logs for Cloud Functions from the [Cloud Console](https://console.cloud.google.com/project/_/logs?service=cloudfunctions.googleapis.com&_ga=2.186241966.-665913479.1577620601)
