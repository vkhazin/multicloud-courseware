# Lab: Terraform Aws, Azure and GCP

## AWS

## Azure

Open [https://shell.azure.com/](https://shell.azure.com/) and login with Microsoft Credentials with access to Azure subscription

Select `bash` shell

Select `Open editor`

In the terminal type `az account list` to confirm a proper authentication 

Create a new folder `azure-terraform` to place the files

Select the `refresh` icon in case folder structure does not reflect the new folder/file

To install terraform:

```
cd ./azure-terraform/ &&
mkdir ./bin &&
wget -O terraform.zip https://releases.hashicorp.com/terraform/0.12.18/terraform_0.12.18_linux_amd64.zip &&
unzip -o terraform.zip -d ./bin &&
rm -f terraform.zip
```

Using the terminal create a new file: `touch ./azure-terraform/provider.tf`

Define the [provider](https://www.terraform.io/docs/providers/index.html) in the new file:

```
provider "azurerm" {
  version = ">= 1.3.3"
}
```

Create a new file: `variables.tf` with the following content:

```
variable "azure_region" {      
  default = "Central US"      
}
```

Create a new file: `resource-group.tf` with the following content:

```
resource "azurerm_resource_group" "resource-group" {  
  name     = "rg-tf-resource-group"  
  location = var.azure_region  
}
```

Create a new file: `vnet.tf` with the following content:

```
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

Create a new file: `security.tf` with the following content:

```
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
```

## GCP



