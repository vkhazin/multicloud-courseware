# Lab: Terraform Aws, Azure and GCP

## AWS

## Azure

1. Open [https://shell.azure.com/](https://shell.azure.com/) and login with Microsoft Credentials with access to Azure subscription
2. Select `bash` shell
3. Select `Open editor` to open file browser
4. In the terminal type `az account list` to confirm a proper authentication 
5. Create a new folder `azure-terraform` to place the files
6. Select the `refresh` icon in case folder structure does not reflect the new folder/file
7. To install terraform:
   ```
   cd ./azure-terraform/ &&
   mkdir ./bin &&
   wget -O terraform.zip https://releases.hashicorp.com/terraform/0.12.18/terraform_0.12.18_linux_amd64.zip &&
   unzip -o terraform.zip -d ./bin &&
   rm -f terraform.zip
   ```
8. Using the terminal create a new file: `touch ./azure-terraform/provider.tf`
9. Define the [provider](https://www.terraform.io/docs/providers/index.html) in the new file:
   ```
   provider "azurerm" {
     version = ">= 1.3.3"
   }
   ```
10. Create a new file: `variables.tf` with the following content:`  
    variable "azure-region" {  
      default = "  
    }`

11. 
12. 
13. 
## GCP



