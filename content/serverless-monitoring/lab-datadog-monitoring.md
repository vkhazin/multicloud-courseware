# Lab: DataDog Serverless Monitoring

## AWS

1. The Serverless dashboard requires no installation of its own, but it relies on three data sources that require their own installation:
2. Set up Amazon Web Services integration which is required, follor the steps:
3. Setting up the Datadog integration with Amazon Web Services requires configuring role delegation using AWS IAM
4. Create a new role in the [AWS IAM Console](https://console.aws.amazon.com/iam/home#/roles)
5. Select `Another AWS account` for the Role Type
6. For Account ID, enter <Your Data Dog Account Id> (Datadog’s account ID). This means that you are granting Datadog read only access to your AWS data
7. Check off `Require external ID` and enter the one generated in the [Datadog app](https://app.datadoghq.com/account/settings#integrations/amazon_web_services). Make sure you leave **Require MFA** disabled. For more information about the External ID, refer to this document in the IAM User Guide
8. Click `Next: Permissions`
9. If you’ve already created the policy, search for it on this page and select it, then skip to step 12. Otherwise, click `Create Policy`, which opens in a new window
10. Select the `JSON` tab. To take advantage of every AWS integration offered by Datadog, use [policy snippet](https://docs.datadoghq.com/integrations/amazon_web_services/?tab=allpermissions#datadog-aws-iam-policy) below in the textbox. As other components are added to an integration, these permissions may change
11. Click `Review policy`
12. Name the policy `DatadogAWSIntegrationPolicy` or one of your own choosing, and provide an apt description
13. Click `Create policy`. You can now close this window
14. Back in the “Create role” window, refresh the list of policies and select the policy you just created
15. Click `Next: Review`
16. Set up **[DATADOG AWS IAM POLICY](https://docs.datadoghq.com/integrations/amazon_web_services/?tab=allpermissions#datadog-aws-iam-policy)**
17. Core Permissions: 
18. ```
    {
      "Version": "2012-10-17",
      "Statement": [
        {
          "Action": [
            "cloudwatch:Get*",
            "cloudwatch:List*",
            "ec2:Describe*",
            "support:*",
            "tag:GetResources",
            "tag:GetTagKeys",
            "tag:GetTagValues"
          ],
          "Effect": "Allow",
          "Resource": "*"
        }
      ]
    }
    ```
19. All Perimissions configurtaion:
20. ```
    {
      "Version": "2012-10-17",
      "Statement": [
        {
          "Action": [
            "apigateway:GET",
            "autoscaling:Describe*",
            "budgets:ViewBudget",
            "cloudfront:GetDistributionConfig",
            "cloudfront:ListDistributions",
            "cloudtrail:DescribeTrails",
            "cloudtrail:GetTrailStatus",
            "cloudwatch:Describe*",
            "cloudwatch:Get*",
            "cloudwatch:List*",
            "codedeploy:List*",
            "codedeploy:BatchGet*",
            "directconnect:Describe*",
            "dynamodb:List*",
            "dynamodb:Describe*",
            "ec2:Describe*",
            "ecs:Describe*",
            "ecs:List*",
            "elasticache:Describe*",
            "elasticache:List*",
            "elasticfilesystem:DescribeFileSystems",
            "elasticfilesystem:DescribeTags",
            "elasticloadbalancing:Describe*",
            "elasticmapreduce:List*",
            "elasticmapreduce:Describe*",
            "es:ListTags",
            "es:ListDomainNames",
            "es:DescribeElasticsearchDomains",
            "health:DescribeEvents",
            "health:DescribeEventDetails",
            "health:DescribeAffectedEntities",
            "kinesis:List*",
            "kinesis:Describe*",
            "lambda:AddPermission",
            "lambda:GetPolicy",
            "lambda:List*",
            "lambda:RemovePermission",
            "logs:TestMetricFilter",
            "logs:PutSubscriptionFilter",
            "logs:DeleteSubscriptionFilter",
            "logs:DescribeSubscriptionFilters",
            "rds:Describe*",
            "rds:List*",
            "redshift:DescribeClusters",
            "redshift:DescribeLoggingStatus",
            "route53:List*",
            "s3:GetBucketLogging",
            "s3:GetBucketLocation",
            "s3:GetBucketNotification",
            "s3:GetBucketTagging",
            "s3:ListAllMyBuckets",
            "s3:PutBucketNotification",
            "ses:Get*",
            "sns:List*",
            "sns:Publish",
            "sqs:ListQueues",
            "support:*",
            "tag:GetResources",
            "tag:GetTagKeys",
            "tag:GetTagValues",
            "xray:BatchGetTraces",
            "xray:GetTraceSummaries"
          ],
          "Effect": "Allow",
          "Resource": "*"
        }
      ]
    }
    ```
21. Set up **Role Delegation**
22. Open the [AWS integration tile](https://app.datadoghq.com/account/settings#integrations/amazon_web_services)
23. Select the Role Delegation tab
24. Enter your AWS Account ID without dashes, `e.g. 123456789012`. Your Account ID can be found in the ARN of the role created during the `[installation of the AWS integration](https://docs.datadoghq.com/integrations/amazon_web_services/?tab=roledelegation#installation)`
25. Choose the services you want to collect metrics for and Click `Install Integration`
26. Now, we are ready to set up the *Datadog Lambda function*
27. You may create the Lambda sunction manually or use the [AWS Serverless Repository](https://serverlessrepo.aws.amazon.com/applications/arn:aws:serverlessrepo:us-east-1:464622532012:applications~Datadog-Log-Forwarder) to deploy the Lambda in your AWS account
28. Once done, go into your [Datadog Log section](https://app.datadoghq.com/logs) to start exploring your logs!

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
