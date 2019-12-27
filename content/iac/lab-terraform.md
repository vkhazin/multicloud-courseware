# Lab: Terraform Aws, Azure and GCP

## AWS

## Azure

1. Open [https://shell.azure.com/](https://shell.azure.com/) and login with Microsoft Credentials with access to Azure subscription
2. Select `bash` shell
3. Select `Open editor`
4. In the terminal type `az account list` to confirm a proper authentication
5. Create a new folder `azure-terraform` to place the files
6. Select the `refresh` icon in case folder structure does not reflect the new folder/file
7. To install terraform:
8. ```
   cd ./azure-terraform/ &&
   mkdir ./bin &&
   wget -O terraform.zip https://releases.hashicorp.com/terraform/0.12.18/terraform_0.12.18_linux_amd64.zip &&
   unzip -o terraform.zip -d ./bin &&
   rm -f terraform.zip
   ```
9. Using the terminal create a new file: `touch ./azure-terraform/provider.tf`
10. Define the [provider](https://www.terraform.io/docs/providers/index.html) in the new file:
11. ```
    provider "azurerm" {
      version = ">= 1.3.3"
    }
    ```
12. Create a new file: `variables.tf` with the following content:
13. ```
    variable "azure_region" {      
      default = "Central US"      
    }
    ```
14. Create a new file: `resource-group.tf` with the following content:
15. ```
    resource "azurerm_resource_group" "resource-group" {  
      name     = "rg-tf-resource-group"  
      location = var.azure_region  
    }
    ```
16. Create a new file: `vnet.tf` with the following content:
17. ```
    resource "azurerm_virtual_network" "vnet" {
       address_space = ["10.0.0.0/16"]
       location = var.azure_region
       name = "vn-primary"
       resource_group_name = azurerm_resource_group.resource-group.name
     }

     resource "azurerm_subnet" "subnet" {
        address_prefix = "10.0.0.0/24"
        name = "sn-public"
        resource_group_name = azurerm_resource_group.resource-group.name
        virtual_network_name = azurerm_virtual_network.vnet.name
     }
    ```
18. Create a new file: `security.tf` with the following content:
19. ```
    resource "azurerm_network_security_group" "sg-public" {
       location = var.azure_region
       name = "sg-public"
       resource_group_name = azurerm_resource_group.resource-group.name
     }

     resource "azurerm_network_security_rule" "sr-public" {
       access = "allow"
       direction = "Inbound"
       name = "ssh"
       network_security_group_name = azurerm_network_security_group.sg-public.name
       priority = 100
       protocol = "tcp"
       source_port_range = "22"
       destination_port_range = "22"
       source_address_prefixes = ["0.0.0.0"]
       destination_address_prefixes = ["0.0.0.0"]
       resource_group_name = azurerm_resource_group.resource-group.name
     }

    resource "azurerm_subnet_network_security_group_association" "sg-sb-association" {
       network_security_group_id = azurerm_network_security_group.sg-public.id
       subnet_id = azurerm_subnet.subnet.id
     }

    provider "tls" {}

    resource "tls_private_key" "key" {
      algorithm = "RSA"
      rsa_bits = 2048
    }

    resource "local_file" "private_key" {
      filename = "ssh-private-key.pem"
      content = tls_private_key.key.private_key_pem
    }
    ```
20. Create a new file: `ubuntu-vn.tf` with the following content:
21. ```
    resource "azurerm_public_ip" "public_ip" {
      location = var.azure_region
      name = "ubuntu-ip"
      resource_group_name = azurerm_resource_group.resource-group.name
      allocation_method = "Dynamic"
    }

    resource "azurerm_network_interface" "nic" {
      location = var.azure_region
      name = "nic-ubuntu"
      resource_group_name = azurerm_resource_group.resource-group.name
      ip_configuration {
        name = "ubuntu-vm-ip"
        private_ip_address_allocation = "Dynamic"
        public_ip_address_id = azurerm_public_ip.public_ip.id
        subnet_id = azurerm_subnet.subnet.id
      }
    }
    ```
22. Validate the template:
23. ```
    ./bin/terraform init &&
    ./bin/terraform validate
    ```
24. Address any issues reported
25. Apply the changes to Azure: `./bin/terraform apply --auto-approve`
26. Expected outcome:
27. ```
    ...
    Apply complete! Resources: 10 added, 0 changed, 0 destroyed.
    ```
28. Open [Azure Portal](https://portal.azure.com) and explore the resources created
29. When satisfied we can remove the deployment: `./bin/terraform destroy --auto-approve`
30. Expected outcome: `Destroy complete! Resources: 10 destroyed.`

## GCP



