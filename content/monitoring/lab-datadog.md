# Lab: Datadog Monitoring

In this lab, we will enroll previously [terraform provisioned VM's](/../iac/lab-terraform.md) into Datadog monitoring

First, we will need an API key from the Datadog: [https://app.datadoghq.com/account/settings\#api](https://app.datadoghq.com/account/settings#api)

Take a note of the key we will need it in the following steps

## AWS

1. Relaunch Cloud9 environment
1. Reprovision the Ubuntu VM using terraform or re-use previously provisioned VM
1. Login into the VM provisioned using ssh:
1. ```
  public_ip=$(./bin/terraform output ubuntu_public_ip)
  ssh -i ./courseware-terraform.pem ubuntu@${public_ip}
  ```
1. Execute inside the VM:
1. ```
  DD_AGENT_MAJOR_VERSION=7 \
  DD_API_KEY={Your DataDog API KEY} \
  bash -c "$(curl -L https://raw.githubusercontent.com/DataDog/datadog-agent/master/cmd/agent/install_script.sh)"
  ```
1. To check integration is working, navigate to DataDog `Infrastructure` -> `All Hosts`, after a few minutes the first host should show up

# Azure

1. In the Azure portal, navigate to the `VM` -&gt; `Extensions` -&gt; `Add`
1. Select `Datadog Agent`
1. Select `Create`, paste your Datadog API key, and select `OK`
1. There is also a terraform template for VM extensions: [azurerm\_virtual\_machine\_extension](https://www.terraform.io/docs/providers/azurerm/r/virtual_machine_extension.html) if you wonder how would you automate the setup
1. It will take a few minutes for the extension to get installed and a bit longer before the VM appears on the host map in DataDog

## GCP

1. Open GCP console [https://console.cloud.google.com](https://console.cloud.google.com)
1. Select the project where a VM has been deployed using terraform
1. Navigate to `IAM` -&gt; `Service Accounts`
1. Create a new service account: `datadog`
1. For the account roles add 3 roles:
1. `Compute engine` —&gt; `Compute Viewer`
1. `Monitoring` —&gt; `Monitoring Viewer`
1. `Cloud Asset` —&gt; `Cloud Asset Viewer`
1. Select `Continue`
1. And create a JSON key - you will need to enable integration on Datadog
1. Navigate to [Datadog GCP Integration](https://app.datadoghq.com/account/settings#integrations/google_cloud_platform) -&gt; `Configuration` tab
1. Upload the service account key and complete the registration
1. It will take a few minutes before data appears on the DataDog -&gt; Metrics -&gt; gcp\*
1. You can plot CPU, Memory, and Disk metrics on a dashboard as you would expect from any infrastructure monitoring solution



