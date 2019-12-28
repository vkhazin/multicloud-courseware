# Lab: Azure Monitor

Open a web browser to [https://shell.azure.com/](https://shell.azure.com/) and login with Microsoft Credentials with access to Azure subscription

Select `bash` shell

Select `Open editor`

Authenticate to az cli: `az login` and follow the instructions

Apply the terraform templates: cd ./azure-terraform &&`./bin/terraform apply --auto-approve`

Expected outcome:

```
...
Apply complete! Resources: 10 added, 0 changed, 0 destroyed.
```

Proceed to [Azure Portal](https://portal.azure.com/) and verify the VM is running

Select `Home` and find `Azure Monitor` link

Select `Activity Link` on the left hand to see log for terraform `apply` action

Let's collect logs from the VM we have deployed

Azure Portal console is changing frequently, [here](https://docs.microsoft.com/en-us/azure/azure-monitor/learn/quick-collect-azurevm) are the latest steps to configure the log collection from VM

Please follow the steps described in Microsoft Azure Documentation until you are able to see logs collected from the deployed VM

