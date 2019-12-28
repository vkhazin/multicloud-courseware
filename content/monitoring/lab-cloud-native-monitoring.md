# Lab: Cloud-Native Monitoring

## Aws

1. Open a web browser to [https://console.aws.amazon.com](https://console.aws.amazon.com)
2. From the services select `Cloud9` and launch the environment previously used for terraform
3. Apply the terraform templates: cd ./aws-terraform &&`./bin/terraform apply --auto-approve`
4. Expected outcome:
5. ```
   ...
   Apply complete! Resources: X added, 0 changed, 0 destroyed.
   ```
6. Using console navigate to `CloudWatch` -&gt; `EC2 Dashboard`
7. Explore metrics readily available
8. Select `Dashboards` -&gt; `Create dashboard` -&gt; `Line Widget` -&gt; `Configure`
9. Find `EC2` -&gt; `CPUUtilization` to add to the widget
10. Add as many other metrics as you choose and save the dashboard
11. To collect syslog an [agent](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Install-CloudWatch-Agent.html) will be required

## Azure

1. Open a web browser to [https://shell.azure.com/](https://shell.azure.com/) and login with Microsoft Credentials with access to Azure subscription
2. Select `bash` shell
3. Select `Open editor`
4. Authenticate to az cli: `az login` and follow the instructions
5. Apply the terraform templates: cd ./azure-terraform &&`./bin/terraform apply --auto-approve`
6. Expected outcome:
7. ```
   ...
   Apply complete! Resources: X added, 0 changed, 0 destroyed.
   ```
8. Proceed to [Azure Portal](https://portal.azure.com/) and verify the VM is running
9. Select `Home` and find `Azure Monitor` link
10. Select `Activity Link` on the left hand to see log for terraform `apply` action
11. Let's collect logs from the VM we have deployed
12. Azure Portal console is changing frequently, [here](https://docs.microsoft.com/en-us/azure/azure-monitor/learn/quick-collect-azurevm) are the latest steps to configure the log collection from VM
13. Please follow the steps described in Microsoft Azure Documentation
14. What we expect to see is `Heartbeat` and `Perf` logs collected in `Log Analytics workspace`
15. Note the common mistake: not saving the changes
16. And there is a delay in collecting the logs, rebooting the VM may help
17. Explorer `Connected Sources`, there is a way to send logs from any Linux host by installing an agent



