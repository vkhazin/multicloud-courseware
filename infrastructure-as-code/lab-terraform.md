# Lab: Terraform

### _Keep all the files - we will need them in the next labs_

## AWS

1. Open a web browser to [https://console.aws.amazon.com](https://console.aws.amazon.com)
2. From the services select `Cloud9`, please note you may need to select a region where Cloud9 is available
3. Create a new environment with any name, choose micro or small instance type and Amazon Linux for the platform
4. Wait until the environment is created and loads editor in your browser
5. To install terraform run the following commands, using terminal panel of Cloud9 IDE:
6. ```text
   mkdir ./aws-terraform && 
   cd ./aws-terraform/ && 
   mkdir ./bin && 
   wget -O terraform.zip https://releases.hashicorp.com/terraform/0.12.18/terraform_0.12.18_linux_amd64.zip && 
   unzip -o terraform.zip -d ./bin &&
   rm -f terraform.zip
   ```
7. Create new [variables](https://www.terraform.io/docs/configuration/variables.html) file: `./aws-terraform/variables.tf` with the following content:
8. ```text
   variable "aws_region" {
       default = "us-east-1"
   }

   variable "aws_availability_zone" {
     default = "us-east-1a"
   }

   variable "key_name" {
     default = "courseware-terraform"
   }

   output "ubuntu_public_ip" {
     value = aws_instance.ubuntu-vm.public_ip
   }
   ```
9. Define the [provider](https://www.terraform.io/docs/providers/index.html) in a new file `./aws-terraform/provider.tf`:
10. ```text
    provider "aws" {
      region     = var.aws_region
    }
    ```
11. Create a new file for network related resources `./aws-terraform/network.tf`:
12. ```text
    resource "aws_vpc" "vpc" {
      cidr_block           = "10.0.0.0/16"
      enable_dns_hostnames = true
      tags = {
        Name = "Dev Vpc"
      }
    }

    resource "aws_subnet" "public-subnet" {
      vpc_id              = aws_vpc.vpc.id
      cidr_block          = "10.0.0.0/24"
      availability_zone   = var.aws_availability_zone
      tags = {
        Name = "Dev Public Subnet"
      }
    }

    resource "aws_internet_gateway" "igw" {
      vpc_id = aws_vpc.vpc.id
    }

    resource "aws_route_table" "route-table" {
      vpc_id = aws_vpc.vpc.id

      route {
        cidr_block = "0.0.0.0/0"
        gateway_id = aws_internet_gateway.igw.id
      }

      tags = {
        Name = "Dev Vpc Route Table"
      }
    }

    resource "aws_route_table_association" "rt-association" {
      subnet_id      = aws_subnet.public-subnet.id
      route_table_id = aws_route_table.route-table.id
      depends_on = [
        aws_subnet.public-subnet,
        aws_route_table.route-table
      ]
    }
    ```
13. Create a new file for security related resources `./aws-terraform/security.tf`:
14. ```text
    resource "tls_private_key" "ssh-key" {
        algorithm = "RSA"
        rsa_bits = 4096
    }

    resource "local_file" "ssh-key" {
        content             = tls_private_key.ssh-key.private_key_pem
        filename            = "./${var.key_name}.pem"
        file_permission     = "400"
    }

    resource "aws_key_pair" "ssh-key-pair" {
        key_name = var.key_name
        public_key = tls_private_key.ssh-key.public_key_openssh
    }

    resource "aws_security_group" "sg" {
        name = "Dev Security Group"
        description = "Allow SSH inbound traffic"
        vpc_id = aws_vpc.vpc.id
    }

    resource "aws_security_group_rule" "ssh_inbound_access" {
        from_port = 22
        protocol = "tcp"
        security_group_id = aws_security_group.sg.id
        to_port = 22
        type = "ingress"
        cidr_blocks = [
            "0.0.0.0/0"
        ]
    }

    resource "aws_security_group_rule" "outbound_access" {
        protocol = "-1"
        security_group_id = aws_security_group.sg.id
        from_port = 0
        to_port = 0
        type = "egress"
        cidr_blocks = [
            "0.0.0.0/0"
        ]    
    }
    ```
15. Create a new file: `ubuntu-vm.tf` with the following content:
16. ```text
    data "aws_ami" "instance_ami" {
      most_recent = true
      owners      = [
        "099720109477"
      ]

      filter {
        name   = "name"
        values = ["ubuntu/images/hvm-ssd/ubuntu-bionic-18.04-amd64-server-*"]
      }

      filter {
        name   = "virtualization-type"
        values = ["hvm"]
      }

      filter {
        name   = "root-device-type"
        values = ["ebs"]
      }
    }

    resource "aws_instance" "ubuntu-vm" {
      ami                         = data.aws_ami.instance_ami.id
      instance_type               = "t3.micro"
      subnet_id                   = aws_subnet.public-subnet.id
      vpc_security_group_ids      = [
        aws_security_group.sg.id
      ]
      associate_public_ip_address = true
      key_name                    = aws_key_pair.ssh-key-pair.key_name
    }
    ```
17. Validate the templates: `./bin/terraform init && ./bin/terraform validate`
18. Address any issues reported
19. Apply the changes: `./bin/terraform apply --auto-approve`
20. Expected output:
21. ```text
    ...
    Apply complete! Resources: X added, 0 changed, 0 destroyed.
    ```
22. Open [Aws Console](https://console.aws.amazon.com) and explore the resources created
23. When satisfied we can remove the deployment: `./bin/terraform destroy --auto-approve`
24. Expected output:
25. ```text
    ...
    Destroy complete! Resources: X destroyed.
    ```

## Azure

1. Open a web browser to [https://shell.azure.com/](https://shell.azure.com/) and login with Microsoft Credentials with access to Azure subscription
2. May need to create a new storage account if that's your first time here, give it a moment or two
3. After the shell has been loaded
4. Select `bash` shell
5. Select `Open editor`icon from the tool bar
6. Authenticate to az cli using terminal panel: `az login` and follow the instructions
7. In the terminal type `az account list` to confirm a proper authentication
8. Create a new folder `mkdir azure-terraform` to place the files
9. Select the `refresh` icon in case folder structure does not reflect the new folder/file
10. Terraform is pre-installed on Azure Shell
12. Using the terminal create a new file: `touch ./azure-terraform/provider.tf`
13. Define the [provider](https://www.terraform.io/docs/providers/index.html) in a new file:
14. ```text
    provider "azurerm" {
      version = ">= 1.3.3"
      features {}
    }
    ```
15. Create a new file: `variables.tf` with the following content:
16. ```text
    variable "azure_region" {      
      default = "Central US"      
    }

    output "ubuntu_public_ip" {
     value = azurerm_public_ip.public_ip.ip_address
    }
    
    ```
17. Create a new file: `resource-group.tf` with the following content:
18. ```text
    resource "azurerm_resource_group" "resource-group" {  
      name     = "rg-tf-resource-group"  
      location = var.azure_region  
    }
    ```
19. Create a new file: `vnet.tf` with the following content:
20. ```text
    resource "azurerm_virtual_network" "vnet" {
       address_space = ["10.0.0.0/16"]
       location = var.azure_region
       name = "vn-primary"
       resource_group_name = azurerm_resource_group.resource-group.name
     }

     resource "azurerm_subnet" "subnet" {
        address_prefixes = ["10.0.0.0/24"]
        name = "sn-public"
        resource_group_name = azurerm_resource_group.resource-group.name
        virtual_network_name = azurerm_virtual_network.vnet.name
     }
    ```
21. Create a new file: `security.tf` with the following content:
22. ```text
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
       source_port_range = "*"
       destination_port_range = "22"
       source_address_prefix = "*"
       destination_address_prefix = "VirtualNetwork"
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
23. Create a new file: `ubuntu-vm.tf` with the following content:
24. ```text
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

    resource "azurerm_virtual_machine" "ubuntu-vm" {
      location = var.azure_region
      name = "ubuntu-vm"
      network_interface_ids = [
        "${azurerm_network_interface.nic.id}"
      ]
      resource_group_name = azurerm_resource_group.resource-group.name
      vm_size = "Standard_B1s"
      storage_os_disk {
        name              = "vmdist"
        caching           = "ReadWrite"
        create_option     = "FromImage"
        managed_disk_type = "Standard_LRS"
      }
      storage_image_reference {
        publisher = "Canonical"
        offer     = "UbuntuServer"
        sku       = "18.04-LTS"
        version   = "latest"
      }
      os_profile {
        admin_username = "ubuntu"
        computer_name = "ubuntu"
      }
      os_profile_linux_config {
        disable_password_authentication = true
        ssh_keys {
          key_data = tls_private_key.key.public_key_openssh
          path = "/home/ubuntu/.ssh/authorized_keys"
        }
      }
    }
    ```
25. Validate the templates: `./bin/terraform init && ./bin/terraform validate`
26. Address any issues reported
27. Apply the changes: `./bin/terraform apply --auto-approve`
28. Expected outcome:
29. ```text
    ...
    Apply complete! Resources: X added, 0 changed, 0 destroyed.
    ```
30. Open [Azure Portal](https://portal.azure.com) and explore the resources created
31. You can log in into the VM using ssh shell: `chmod 400 ./ssh-private-key.pem && ssh -i ./ssh-private-key.pem ubuntu@{public-ip}`
32. When satisfied we can remove the deployment: `./bin/terraform destroy --auto-approve`
33. Expected outcome:
34. ```text
    ...
    Destroy complete! Resources: X destroyed.
    ```

## GCP

1. Open a web browser to [https://console.cloud.google.com/](https://console.cloud.google.com/) and authenticate
2. GCP API's are eventually consistent we may need to wait here and there before we continue
3. Create a [new project](https://cloud.google.com/resource-manager/docs/creating-managing-projects) with the name: `gcp-terraform`
4. Navigate to the project home and copy `Project ID`, different from the project name, can be found on the project dashboard page
5. Wait for the creation process to complete...
6. Navigate to `Compute Engine` and wait for the API getting enabled...
7. Select `Activate Cloud Shell`
8. Select `Launch Editor`
9. Terraform is pre-installed on GCP Shell
10. Generate a new ssh key: `ssh-keygen -b 2048 -t rsa -f gcpadmin-key -q -N ""`
11. Create a new file: `variables.tf` with the following content:
12. ```text
    variable "project_id" {
      default = "gcp-terraform" <-- replace with your own project id
    }

    variable "gcp_region" {
      default = "us-central1"
    }
    
    output "ubuntu_public_ip" {
      value = google_compute_instance.default.network_interface.0.access_config.0.nat_ip
    }    
    ```
13. Define [provider](https://www.terraform.io/docs/providers/index.html) in a new file `./gcp-terraform/provider.tf`:
14. ```text
    provider "google" {
      project = var.project_id
    }
    ```
15. Create a new file `./gcp-terraform/network.tf`:
16. ```text
    resource "google_compute_network" "network" {
      name                    = "network"
      auto_create_subnetworks = false
      routing_mode            = "GLOBAL"
      project                 = var.project_id
    }

    resource "google_compute_subnetwork" "subnetwork" {
      name          = "subnetwork"
      ip_cidr_range = "10.0.0.0/24"
      region        = var.gcp_region
      network       = google_compute_network.network.name
      project       = var.project_id
    }
    ```
17. Create a new file `./gcp-terraform/security.tf`:
18. ```text
    resource "google_compute_firewall" "allow-tag-ssh" {
      name          = "${google_compute_network.network.name}-ingress-tag-ssh"
      description   = "Allow SSH to machines with 'ssh' tag"
      network       = google_compute_network.network.name
      project       = var.project_id
      source_ranges = ["0.0.0.0/0"]
      target_tags   = ["ssh"]

      allow {
        protocol = "tcp"
        ports    = ["22"]
      }
    }
    ```
19. Create a new file `./gcp-terrafrom/ubuntu-vm.tf`:
20. ```text
    resource "google_compute_instance" "default" {
      name         = "ubuntu-vm"
      machine_type = "f1-micro"
      zone         = "us-central1-a"

      boot_disk {
        initialize_params {
          image = "ubuntu-os-cloud/ubuntu-1804-lts"
        }
      }
      network_interface {
        subnetwork = google_compute_subnetwork.subnetwork.name
        access_config {
          // Include this section to give the VM an external ip address
        }
      }
      metadata = {
        sshKeys = "gcpadmin:${file("gcpadmin-key.pub")}"
      }

      // Apply the firewall rule to allow external IPs to access this instance
      tags = ["ssh"]
    }
    ```
21. Validate the templates:
22. ```text
    ./bin/terraform init &&
    ./bin/terraform validate
    ```
23. Address any issues reported
24. Apply the changes: `./bin/terraform apply --auto-approve`
25. Expected outcome:
    ```text   
    Apply complete! Resources: X added, 0 changed, 0 destroyed.

    Outputs:

    ubuntu_public_ip = XXX.XXX.XXX.XXX 
    ```
26. Open [GCP Console](https://console.cloud.google.com) and explore the resources created

27. When satisfied we can remove the deployment: `./bin/terraform destroy --auto-approve`

28. Expected outcome:
    ```text
        Destroy complete! Resources: X destroyed.
    ```

### Congratulations, you have automated deployment of a VM on three different cloud providers!

