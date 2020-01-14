# Lab: DataDog PaaS Monitoring


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
