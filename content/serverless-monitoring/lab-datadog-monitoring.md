# Lab: DataDog Serverless Monitoring


## Azure

1. DataDog provides Azure Integration, which includes a wide area of supported azure services including, PaaS, IaaS, etc.
2. Run this Script in Azure CLI to get `appID`
3.  ```
    # First, log in to the Azure account you want to integrate with Datadog:
    az login

    # Run the account show command:
    az account show

    # Create an application as a service principal using the format:
    az ad sp create-for-rbac --role reader --scopes /subscriptions/{subscription_id}

    # This command grants the Service Principal the reader role for the subscription you would like to monitor.
    ```
4. The `appID` generated from this command must be entered in the [Datadog Azure Integration](https://app.datadoghq.com/account/settings#integrations/azure) tile under `Client ID`.
5. Add `--name <CUSTOM_NAME>` to use a hand-picked name, otherwise Azure generates a unique one. The name is not used in the setup process.
6. Add `--password <CUSTOM_PASSWORD>` to use a hand-picked password. Otherwise Azure generates a unique one. This password must be entered in the [Datadog Azure Integration](https://app.datadoghq.com/account/settings#integrations/azure) tile under `Client Secret`.
7. **Note:** The Azure Functions integration does not include any *events* and *service checks*. 

## GCP

1. We need to set up the [Google Cloud Platform integration](https://docs.datadoghq.com/integrations/google_cloud_platform) first, if already setup you may start following from *11* tp setup Cloud pub/sub
2. So, creating a service account and providing Datadog with service account credentials:
3. Navigate to the [Google Cloud credentials page](https://console.cloud.google.com/apis/credentials) for the Google Cloud project where you would like to setup the Datadog integration
4. Press Create credentials and then select Service account key
5. In the Service account dropdown, select New service account
6. For *Role*, select *Compute engine* —> *Compute Viewer*, *Monitoring* —> *Monitoring Viewer*, and *Cloud Asset* —> *Cloud Asset Viewer*
7. Select *JSON* as the key type, and press create. Take note where this file is saved, as it is needed to complete the installation
8. Navigate to the [Datadog Google Cloud Integration tile](http://app.datadoghq.com/account/settings#integrations/google_cloud_platform)
9. On the Configuration tab, select Upload Key File to integrate this project with Datadog
10. Optionally, you can use tags to filter out hosts from being included in this integration
11. Now, Set up a Cloud pub/sub with an HTTP push forwarder
12. Go to the [Cloud Pub Sub console](https://console.cloud.google.com/cloudpubsub/topicList) and create a new topic
13. Give that topic an explicit name such as `export-logs-to-datadog` and Save
14. Configure pub/sub to forward pass logs to DataDog:
15. Go back to the Pub/Sub that was previously created, and add a new `subscription`:
16. ```
    # For DataDog US Site
    Select the Push method and enter the following: https://gcp-intake.logs.datadoghq.com/v1/input/<DATADOG_API_KEY>/
    # For DataDog EU Site
    Select the Push method and enter the following: https://gcp-intake.logs.datadoghq.eu/v1/input/<DATADOG_API_KEY>/
    ```
17. Hit `Create` at the bottom
18. The Pub/Sub is now ready to receive logs from Stackdriver and forward them to Datadog
18. Export logs from stackdriver to the pub/sub:
19. Go to [the Stackdriver page](https://console.cloud.google.com/logs/viewer) and filter the logs that need to be exported
20. Hit `Create Export` and name the sink accordingly
21. Choose `Pub/Sub` as the destination and select the Pub/Sub that was created for that purpose. Note that the Pub/Sub can be located in a different project
22. Hit `Create` and wait for the confirmation message to show up
23. **Note**: Pub/subs are subject to [Google Cloud quotas and limitations](https://cloud.google.com/pubsub/quotas#quotas). If the number of logs you have is higher than those limitations, Datadog recommends you split your logs over several subscriptions. See the [Monitor the Log Forwarding](https://docs.datadoghq.com/integrations/google_cloud_platform/?tab=datadogeusite#monitor-the-log-forwarding) section for more information
