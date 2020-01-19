# Lab: DataDog PaaS Monitoring

## AWS

1. To configure the integration between Aws and DataDog we will need to grant read-only access to Datadog app account
2. Navigate to IAM from [https://console.aws.amazon.com](https://console.aws.amazon.com)
3. Proceed to `Roles` and select `Create role`
4. Select `Another AWS account` for the Role Type
5. For the account Id enter `464622532012`, a no-secret DataDog AWS account found in DataDog documentation
6. Enable `Require external ID`
1. Open [DataDog AWS Integration](https://app.datadoghq.com/account/settings#integrations/amazon_web_services) to copy `AWS External ID`
1. Paste the copied id into the `External ID` field in Aws Console
7. Make sure you leave `Require MFA` disabled!
8. Select `Nex: Permissions`
9. Select `Create Policy` and it will open a new browser tab/window -&gt; `JSON tab` -&gt; paste the [policy snippet from DataDog](https://docs.datadoghq.com/integrations/amazon_web_services/?tab=allpermissions#datadog-aws-iam-policy):
10. ```
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
11. Select `Review Policy`
12. Provide a name for the policy: `DatadogAWSIntegrationPolicy`  and select `Create Policy`
13. Back to the previous browser tab/window and refresh the page
14. Search and select the newly created policy
15. Proceed to the next steps to enter the role name: `DatadogAWSIntegrationRole`
16. Complete the setup on [Datadog console](https://app.datadoghq.com/account/settings#integrations/amazon_web_services) AWS Integration Tile
17. Enter your `AWS Account ID` without dashes, e.g. `123456789012` and `Aws Role name` e.g. `DatadogAWSIntegrationRole`
18. Your Account ID can be found in the ARN of the role created during the setup of the role e.g.: `arn:aws:iam::123456789012:role/DatadogAWSIntegrationRole` where the numbers are the account number and `DatadogAWSIntegrationRole` is the role name
19. Select `Install Integration` and give it a moment to finish until you see `AWS Integration successfully updated.` message
20. To receive `Elastic Beanstalk` metrics, we must enable the Enhanced Health Reporting feature for your environment
21. Open the Aws Elastic Beanstalk console
22. Navigate to the management page for your application and environment
23. Select `Configuration`
24. Scroll to, **don't search for!**, `Monitoring` configuration category and choose `Modify`
25. Under `Health reporting`, for System, choose `Enhanced`
26. Select all or some `CloudWatch Custom metrics`
27. Enable `Log Streaming` 
28. Select `Apply`
29. Once configuration changes have been applied, you can go to Datadog `Metrics Explorer` to search for `elasticbeanstalk` metrics

## Azure

1. Open a web browser to [https://shell.azure.com/](https://shell.azure.com/)
2. After the shell has been loaded
3. Select `bash` shell
4. Select `Open editor` icon from the toolbar
5. Run `az login` in the terminal panel and follow the instructions
6. Run `az account show` to obtain subscription id
7. Copy value for `id` field, this is the subscription id we will need in the next command
8. Create a service principal: `az ad sp create-for-rbac --role reader --scopes /subscriptions/{subscription_id}`
9. Open [Datadog Azure Integration](https://app.datadoghq.com/account/settings#integrations/azure)
10. Copy `tenantId` value from `az account show` command into Datadog `Tenant name/ID` column
11. Copy the `appID` generated from `az ad sp create-for-rbac...` command and paste it into the field `Client ID`
12. Copy `password` from the same command into `Client Secret`
13. Enter any desired tenant name e.g. `courseware`
14. Select `Install Integration` and let the installation finish
15. After quite a while, Azure specific metrics will appear in the Datadog Metrics Explorer

## GCP

1. Should you have spare time, you are welcome to update our App Engine deployment based on the following [documentation](https://app.datadoghq.com/account/settings#integrations/google-app-engine)
2. A bummer: Node.js is not on the [list](https://docs.datadoghq.com/integrations/google_app_engine/)



