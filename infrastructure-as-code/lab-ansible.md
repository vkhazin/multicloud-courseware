# Lab: Ansible

### _Keep all the files - we will need them in the next labs_

## AWS / Azure / GCP

1. Open a web browser to [https://console.aws.amazon.com](https://console.aws.amazon.com)

2. From the services select `Cloud9`, please note you may need to select a region where Cloud9 is available

3. Create a new environment with any name, choose micro or small instance type and Amazon Linux for the platform

4. Wait until the environment is created and loads editor in your browser

5. Execute Terraform lab in order to create all the necessary resources (do not execute the last destroy step).

6. [Install ansible] (https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html) by running the following commands, using terminal panel of Cloud9 IDE:

```text   
sudo pip install ansible &&
mkdir ./ansible && 
cd ./ansible/
```

7. Execute the following commands to create the initial folder structure.

```
   touch hosts && 
   touch site.yml &&     
   touch webservers.yml &&         
   mkdir ssh_keys &&
   mkdir group_vars &&
   mkdir -p roles/webservers/tasks &&
   mkdir -p roles/webservers/vars &&
   mkdir -p roles/common/tasks &&
   touch group_vars/all.yml && 
   touch roles/common/tasks/main.yml && 
   touch roles/webservers/tasks/main.yml && 
   touch roles/webservers/vars/main.yml        
```
8. Copy the ssh keys generated in the previous terraform lab into ssh_keys folder and rename them by provider

    * Save AWS courseware-terraform.pem as aws-courseware-terraform.pem in ssh_keys folder and be sure to give the right permissions by running the following command:

   ```
      chmod 400 ./ssh_keys/aws-courseware-terraform.pem
   ```

    * Save Azure ssh-private-key.pem as azure-courseware-terraform.pem in ssh_keys folder and be sure to give the right permissions by running the following command:

   ```
      chmod 400 ./ssh_keys/azure-courseware-terraform.pem
   ```

    * Save GPC gcpadmin-key as gcp-courseware-terraform.pem in ssh_keys folder and be sure to give the right permissions by running the following command:

   ```
      chmod 400 ./ssh_keys/gcp-courseware-terraform.pem
   ```

9. Edit the [inventory file](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html) `./ansible/hosts` and add the following content (replacing xxx.xxx.xxx.xxx with each  corresponding IP address):
   
```
   [webservers]

   # AWS Ubuntu VM public IP obtained from the terraform apply command output  executed in the previous lab.
   xxx.xxx.xxx.xxx ansible_ssh_private_key_file=./ssh_keys/aws-courseware-terraform.pem ansible_ssh_user=ubuntu
   
   # Azure Ubuntu VM public IP obtained from the terraform apply command output executed in the previous lab.
   xxx.xxx.xxx.xxx ansible_ssh_private_key_file=./ssh_keys/azure-courseware-terraform.pem ansible_ssh_user=ubuntu

   # GCP Ubuntu VM public IP obtained from the terraform apply command output executed in the previous lab.                    
   xxx.xxx.xxx.xxx ansible_ssh_private_key_file=./ssh_keys/gcp-courseware-terraform.pem ansible_ssh_user=gcpadmin
```

10. Edit the global variables [group_vars/all.yml](https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html) file (`./ansible/group_vars/all.yml`) to add the following content:

```
ansible_python_interpreter: /usr/bin/python3

```
11. Edit the common [role](https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse_roles.html)  tasks file (`./ansible/roles/common/tasks/main.yml`) with the following content:
```
##### Setup Docker group and user

- name: create docker group
  become: true
  group:
    name: docker
    state: present

- name: add user to group 
  become: true
  user:
    name: "{{ansible_user}}"
    groups: docker
    append: true

- meta: reset_connection

##### Setup Docker

- name: install packages required by docker
  become: true
  apt:
    update_cache: yes
    state: latest
    name:
    - apt-transport-https
    - ca-certificates
    - curl
    - gpg-agent
    - software-properties-common

- name: add docker GPG key
  become: true
  apt_key:
    url: https://download.docker.com/linux/ubuntu/gpg
    state: present

- name: add docker apt repo
  become: true
  apt_repository:
    repo: deb https://download.docker.com/linux/ubuntu bionic stable
    state: present

- name: install docker
  become: true
  apt:
    update_cache: yes
    state: latest
    name:
    - docker-ce
    - docker-ce-cli
    - containerd.io

##### Ansible Setup <---> docker

- name: install python dependencies
  become: true
  apt:
    update_cache: yes
    state: latest
    name: python3-pip

- name: install 'Docker SDK for Python'
  pip:
    name: docker
   ```

12. Open and complete the webservers [role](https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse_roles.html)  tasks (`./ansible/roles/webservers/tasks/main.yml`) with the following content:

```
---
- name: Checkout nodejs endpoint repository
  vars:
    ansible_python_interpreter: /usr/bin/python3
  git:
    repo: "{{ endpoint_repository_url }}"
    dest: "{{ repository_directory }}"
    accept_hostkey: yes
    force: yes
    
- name: Build the endpoint docker image
  docker_image:
    build:
      path: "{{ repository_directory }}"
      pull: yes
    name: "{{ docker_image_repository_path }}"
    tag: "{{ docker_image_tag }}"
    source: build
    
- name: Start docker container
  docker_container:
   name:  "{{ docker_image_name }}"
   image: "{{ docker_image_repository_path }}:{{ docker_image_tag }}"
   state: started
   published_ports: "{{ exposed_host_port }}:{{ docker_container_port }}"
   auto_remove: "yes"
```
13. Open and complete the webservers role variables (`./ansible/roles/webservers/vars/main.yml`) with the following content:

```
endpoint_repository_url       : "https://github.com/vkhazin/courseware-nodejs-container.git"
repository_directory          : "./git/courseware-nodejs-container"
docker_image_name             : node-end-point
docker_image_repository_path  : node/end-point
docker_image_tag              : v1
docker_container_port         : 3001
exposed_host_port             : 3001
```

14. Open and complete the webservers (`./ansible/webservers.yml`) [paybook](https://docs.ansible.com/ansible/latest/user_guide/playbooks_intro.html) with the following content:
```
---
- hosts: webservers     
  roles:
   - { role: common, tags: ['common'] }
   - { role: webservers, tags: ['webserver'] }
```

15. Open and complete the main (`./ansible/site.yml`) [paybook](https://docs.ansible.com/ansible/latest/user_guide/playbooks_intro.html)  with the following content:
```
- import_playbook: webservers.yml
```

16. Run ansible and wait for it to finish

```
export ANSIBLE_HOST_KEY_CHECKING=False; ansible-playbook site.yml -i hosts

```

17. Expected final outcome:

```
export ANSIBLE_HOST_KEY_CHECKING=False; ansible-playbook site.yml -i hosts

```

18. shh into AWS server and verify the container is running:
```
ssh ubuntu@xxx.xxx.xxx.xxx -i ssh_keys/aws-courseware-terraform.pem
curl 'http://localhost:3001?name=John'
```
Expected Output
```
{"message":"Hello John"}
```

19. shh into Azure server and verify the container is running:
```
ssh ubuntu@xxx.xxx.xxx.xxx -i ssh_keys/azure-courseware-terraform.pem 
curl 'http://localhost:3001?name=John'
```
Expected OUtput
```
{"message":"Hello John"}
```

20. shh into GCP server and verify the container is running:
```
ssh ubuntu@xxx.xxx.xxx.xxx -i ssh_keys/gcp-courseware-terraform.pem 
curl 'http://localhost:3001?name=John'
```
Expected OUtput
```
{"message":"Hello John"}
```

21. Once done, do not forget to execute the last step from the previous terraform lab in order to delete all the resources.

### Congratulations, you have automated the endpoint deployment to three different cloud providers using the same script!