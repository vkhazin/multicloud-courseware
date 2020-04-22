# Lab: Datadog Monitoring

* Register for a free account up to 10 hosts at https://www.datadog.com
* In this lab, we will enroll previously [terraform provisioned VM's](https://github.com/vkhazin/multicloud-courseware/tree/bc247795fd96e19a2f69555a662a579a53c318da/iac/lab-terraform.md) into Datadog monitoring
* First, we will need an API key from the Datadog: [https://app.datadoghq.com/account/settings\#api](https://app.datadoghq.com/account/settings#api)
* Take a note of the key we will need it in the following steps

### AWS

1. Relaunch Cloud9 environment
2. Reprovision the Ubuntu VM using terraform or re-use previously provisioned VM
3. Login into the VM provisioned using ssh:
4. ```text
   public_ip=$(./bin/terraform output ubuntu_public_ip)
   ssh -i ./courseware-terraform.pem ubuntu@${public_ip}
   ```
5. Execute inside the VM:
6. ```text
   DD_AGENT_MAJOR_VERSION=7 \
   DD_API_KEY={Your DataDog API KEY} \
   bash -c "$(curl -L https://raw.githubusercontent.com/DataDog/datadog-agent/master/cmd/agent/install_script.sh)"
   ```
7. To check integration is working, navigate to DataDog `Infrastructure` -&gt; `All Hosts`, after a few minutes the first host should show up

## Azure

1. In the Azure portal, navigate to the `VM` -&gt; `Extensions` -&gt; `Add`
2. Select `Datadog Agent`
3. Select `Create`, paste your Datadog API key, and select `OK`
4. There is also a terraform template for VM extensions: [azurerm\_virtual\_machine\_extension](https://www.terraform.io/docs/providers/azurerm/r/virtual_machine_extension.html) if you wonder how would you automate the setup
5. It will take a few minutes for the extension to get installed and a bit longer before the VM appears on the host map in DataDog

### GCP

1. Open GCP console [https://console.cloud.google.com](https://console.cloud.google.com)
2. Select the project where a VM has been deployed using terraform
3. Navigate to `IAM` -&gt; `Service Accounts`
4. Create a new service account: `datadog`
5. For the account roles add 3 roles:
6. `Compute engine` —&gt; `Compute Viewer`
7. `Monitoring` —&gt; `Monitoring Viewer`
8. `Cloud Asset` —&gt; `Cloud Asset Viewer`
9. Select `Continue`
10. And create a JSON key - you will need to enable integration on Datadog
11. Navigate to [Datadog GCP Integration](https://app.datadoghq.com/account/settings#integrations/google_cloud_platform) -&gt; `Configuration` tab
12. Upload the service account key and complete the registration
13. It will take a few minutes before data appears on the DataDog -&gt; Metrics -&gt; gcp\*
14. You can plot CPU, Memory, and Disk metrics on a dashboard as you would expect from any infrastructure monitoring solution

