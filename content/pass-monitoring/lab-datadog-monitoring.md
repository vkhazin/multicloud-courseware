# Lab: DataDog PaaS Monitoring

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
16. Set up **DATADOG AWS IAM POLICY**
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
26. Once done, go into your [Datadog Log section](https://app.datadoghq.com/logs) to start exploring your logs!

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

## GCP

**Note:** Ensure that Billing is enabled on your Google App Engine project to collect all metrics
1. Change directory into to your project’s application directory
2. Clone the Datadog Google App Engine module
3. `git clone https://github.com/DataDog/gae_datadog`
4. Edit your project’s `app.yaml` file
5. Add the Datadog handler to your `app.yaml` file:
6. 	```
	handlers:
	  # Should probably be at the beginning of the list
	  # so it's not clobbered by a catchall route
	  - url: /datadog
		script: gae_datadog.datadog.app
	```
7. Set your API key. This should be at the top level of the file and not in the handler section
8. 	```
	env_variables:
		DATADOG_API_KEY: '<YOUR_DATADOG_API_KEY>'
	```
9. Since the dogapi module sends metrics and events through a secure TLS connection, add the ssl module in the app.yaml:
10. ```
	libraries:
	  - name: ssl
		version: "latest"
	```
11. Add `dogapi` to the requirements.txt file
12. `echo dogapi >> requirements.txt`
13. Ensure the requirements are installed
14. `pip install -r requirements.txt -t lib/`
15. Deploy your application. Refer to the Google App Engine documentation for language specific deployment commands. For Python apps, it’s:
16. `appcfg.py -A <project id> update app.yaml`
17. Enter the URL for your application in the first text box on the integration configuration screen. If you are using Task queues in the Google Developers Console, you can add them here as well.
18. At this point you will get a number of metrics for your environment
