# Lab: DataDog PaaS Monitoring

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
