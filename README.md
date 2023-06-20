# Linux Server Setup with Terraform and Ansible

This guide provides step-by-step instructions to set up a Linux server in a custom VPC using Terraform and Ansible. The server will be configured with MySQL, Tomcat on port 8080, Memcached, Redis, and a sample WAR file deployment.

## Prerequisites

Before getting started, ensure you have the following prerequisites:

- AWS account credentials with appropriate permissions.
- Terraform installed on your local machine.
- Ansible installed on your local machine.

## Step 1: Set up Terraform

1. Install Terraform by following the official Terraform documentation.
2. Create a new directory for your Terraform configuration files.

## Step 2: Write Terraform Configuration

1. Inside the directory created in the previous step, create a file named `main.tf`.
2. Copy and paste the following configuration into `main.tf`:

```hcl
provider "aws" {
  access_key = "YOUR_AWS_ACCESS_KEY"
  secret_key = "YOUR_AWS_SECRET_KEY"
  region     = "YOUR_AWS_REGION"
}

resource "aws_vpc" "custom_vpc" {
  cidr_block = "10.0.0.0/16"
  # Add any other desired VPC configurations
}

# Define other resources like subnets, security groups, etc., as needed

```

3. Replace the placeholders `YOUR_AWS_ACCESS_KEY`, `YOUR_AWS_SECRET_KEY`, and `YOUR_AWS_REGION` with your AWS credentials and desired region.

## Step 3: Provision the VPC using Terraform

1. Open a terminal and navigate to the directory containing the Terraform files.
2. Run the following commands to initialize and apply the Terraform configuration:

```shell
terraform init
terraform apply
```

Terraform will provision the VPC and any other resources defined in `main.tf` based on the configuration.

## Step 4: Set up Ansible

1. Install Ansible by following the official Ansible documentation.
2. Create a new directory for your Ansible playbook.

## Step 5: Write Ansible Playbook

1. Inside the Ansible playbook directory, create a file named `playbook.yml`.
2. Copy and paste the following playbook into `playbook.yml`:

```yaml
---
- hosts: all
  become: true

  tasks:
    - name: Install packages
      apt:
        name: "{{ item }}"
        state: present
      with_items:
        - mysql-server
        - tomcat8
        - memcached
        - redis-server

    - name: Copy sample WAR file
      copy:
        src: /path/to/sample.war
        dest: /var/lib/tomcat8/webapps/sample.war

    - name: Start Tomcat service
      service:
        name: tomcat8
        state: started

    # Add MySQL, Memcached, Redis configuration and service tasks as needed

```

3. Replace `/path/to/sample.war` with the actual path to your sample WAR file.

## Step 6: Run the Ansible Playbook

1. Open a terminal and navigate to the directory containing the Ansible playbook.
2. Run the following command to execute the playbook:

```shell
ansible-playbook playbook.yml -i <IP_ADDRESS>,
```

3. Replace `<IP_ADDRESS>` with the IP address of your provisioned Linux server.



After running the Ansible playbook, it will install and configure MySQL, Tomcat on port 8080, Memcached, Redis, and deploy the sample WAR file.

Please note that the above instructions provide a basic setup, and you may need to modify the Terraform and Ansible configurations based on your specific requirements.