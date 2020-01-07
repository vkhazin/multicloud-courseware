# Lab: Datadog Monitoring

In this lab we will enroll previously [terraform provisioned VM's](/../iac/lab-terraform.md) into datadog monitoring

First we will need an API key from the Datadog: [https://app.datadoghq.com/account/settings\#api](https://app.datadoghq.com/account/settings#api)

Take a note of the key we will need it in the following steps

## AWS

Login into the VM provisioned using ssh

Replace the key value in the following command

# Azure

In the Azure portal, navigate to the `VM` -&gt; `Extensions` -&gt; `Add` 

Select `Datadog Agent`

Select `Create`, paste your Datadog API key, and select `OK`

There is also a terraform template for VM extensions: [azurerm\_virtual\_machine\_extension](https://www.terraform.io/docs/providers/azurerm/r/virtual_machine_extension.html)

It will take a few minutes for the extension to get installed and a bit longer before the VM appears on the host map in DataDog

## GCP

Open GCP console [https://console.cloud.google.com](https://console.cloud.google.com)

Select the project where a VM has been deployed using terraform

Navigate to `IAM` -&gt; `Service Accounts`

Create a new service account: `datadog`

For roles add 3 roles:

Compute engine —&gt; Compute Viewer

Monitoring —&gt; Monitoring Viewer

Cloud Asset —&gt; Cloud Asset Viewer

Select `Continue`

And create a JSON key - you will need to enable integration on Datadog

Navigate to [Datadog GCP Integration](https://app.datadoghq.com/account/settings#integrations/google_cloud_platform) -&gt; `Configuration` tab

Upload the service account key and complete the registration

It will take a few minutes before data appears on the DataDog -&gt; Metrics -&gt; gcp\*

You can plot CPU, Memory, and Disk metrics as you would expect from any infrastructure monitoring solution





