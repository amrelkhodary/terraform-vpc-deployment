# Terraform AWS VPC Deployment

This repository contains a basic Terraform configuration for deploying an Amazon Web Services (AWS) Virtual Private Cloud (VPC). The configuration follows best practices and is organized with modularity and maintainability in mind.

---

## Table of Contents

- [Overview](#overview)
- [File Structure](#file-structure)
- [Prerequisites](#prerequisites)
- [Usage](#usage)
  - [Initialize the Project](#1-initialize-the-project)
  - [Review the Execution Plan](#2-review-the-execution-plan)
  - [Apply the Configuration](#3-apply-the-configuration)
  - [Verify the Deployment](#4-verify-the-deployment)
  - [Clean Up Resources](#5-clean-up-resources)
- [Configuration](#configuration)
  - [Variables](#variables)
  - [Outputs](#outputs)
- [Best Practices](#best-practices)
- [License](#license)

---

## Overview

This Terraform configuration will create a basic VPC in AWS with the following characteristics:

- A customizable CIDR block.
- Tags for resource identification and management.
- Organized file structure for clarity and scalability.

---

## File Structure

The project is organized into the following files:

```
terraform-vpc/
├── main.tf          # Core resources (VPC definition)
├── variables.tf     # Variable definitions
├── outputs.tf       # Output definitions
├── providers.tf     # Provider configurations
└── versions.tf      # Terraform and provider versions
```

- **`main.tf`**: Contains the resource definitions for the VPC.
- **`variables.tf`**: Defines input variables for customization.
- **`outputs.tf`**: Specifies outputs to display useful information after deployment.
- **`providers.tf`**: Configures the AWS provider.
- **`versions.tf`**: Specifies the required Terraform and provider versions.

---

## Prerequisites

Before you begin, ensure you have the following:

- **Terraform Installed**: [Download and install Terraform](https://www.terraform.io/downloads.html) for your operating system.
- **AWS Account**: An active AWS account with permissions to create VPC resources.
- **AWS Credentials Configured**: Set up your AWS credentials using one of the following methods:
  - **Environment Variables**:
    ```bash
    export AWS_ACCESS_KEY_ID="your-access-key-id"
    export AWS_SECRET_ACCESS_KEY="your-secret-access-key"
    ```
  - **AWS Credentials File**: Located at `~/.aws/credentials`.
  - **AWS CLI Configuration**:
    ```bash
    aws configure
    ```
- **(Optional) AWS CLI Installed**: For verifying resources or additional configurations.

---

## Usage

Follow these steps to deploy the VPC:

### **1. Initialize the Project**

Navigate to the project directory and initialize Terraform. This will download the necessary provider plugins.

```bash
terraform init
```

### **2. Review the Execution Plan**

It's best practice to review the actions Terraform will take before applying changes.

```bash
terraform plan
```

### **3. Apply the Configuration**

Apply the Terraform configuration to create the VPC. You'll be prompted to confirm before proceeding.

```bash
terraform apply
```

When prompted, type `yes` to confirm:

```
Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes
```

### **4. Verify the Deployment**

After the deployment completes, you can verify the VPC has been created:

- **AWS Console**: Log in to the AWS Management Console and navigate to the VPC dashboard to view the new VPC.
- **AWS CLI** (optional):

  ```bash
  aws ec2 describe-vpcs --filters "Name=tag:Name,Values=<your-vpc-name>"
  ```

### **5. Clean Up Resources**

To avoid incurring charges for resources you're not using, destroy the resources when they're no longer needed:

```bash
terraform destroy
```

Confirm by typing `yes` when prompted.

---

## Configuration

### **Variables**

The configuration uses variables to allow customization. You can override the default values by providing variable values in a `terraform.tfvars` file, as command-line arguments, or as environment variables.

#### **Defined Variables:**

- **`aws_region`**: The AWS region where resources will be deployed.
  - **Type**: `string`
  - **Default**: `"us-east-1"`

- **`vpc_cidr_block`**: The CIDR block for the VPC.
  - **Type**: `string`
  - **Default**: `"10.0.0.0/16"`

- **`environment`**: An identifier for the environment (e.g., dev, staging, prod).
  - **Type**: `string`
  - **Default**: `"dev"`

- **`vpc_name`**: The name tag for the VPC.
  - **Type**: `string`
  - **Default**: `"my_vpc"`

#### **Overriding Variables:**

- **Using `terraform.tfvars` File**:

  Create a `terraform.tfvars` file with your variable overrides:

  ```hcl
  aws_region     = "us-west-2"
  vpc_cidr_block = "10.1.0.0/16"
  environment    = "production"
  vpc_name       = "prod_vpc"
  ```

- **Using Command-Line Flags**:

  ```bash
  terraform apply -var="aws_region=us-west-2" -var="vpc_name=prod_vpc"
  ```

- **Using Environment Variables**:

  ```bash
  export TF_VAR_aws_region="us-west-2"
  export TF_VAR_vpc_name="prod_vpc"
  ```

### **Outputs**

After applying, Terraform will display the following outputs:

- **`vpc_id`**: The ID of the created VPC.
- **`vpc_cidr_block`**: The CIDR block of the VPC.

These outputs are defined in the `outputs.tf` file and can be used for integration with other Terraform configurations or for reference.

---

## Best Practices

- **Code Formatting**:

  Ensure consistent code formatting by running:

  ```bash
  terraform fmt -recursive
  ```

- **Validation**:

  Validate the configuration files for syntax errors:

  ```bash
  terraform validate
  ```

- **Version Control**:

  - **Git Ignore**: Use a `.gitignore` file to exclude sensitive files from version control:

    ```gitignore
    # Local .terraform directories
    **/.terraform/*

    # .tfstate files
    *.tfstate
    *.tfstate.*
    crash.log
    *.tfvars

    # Ignore override files
    override.tf
    override.tf.json
    *_override.tf
    *_override.tf.json

    # .terraform plugin directory
    .terraform/

    # Lock file
    .terraform.lock.hcl
    ```

- **State Management**:

  For collaborative environments, consider using a remote backend (like AWS S3 with DynamoDB for state locking) to store the state file.

- **Resource Naming**:

  Use consistent naming conventions for resources and variables to improve readability and maintainability.

- **Comments and Documentation**:

  Include comments in your Terraform files to explain resource configurations and any complex logic.

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

**Note:** This is a basic VPC setup intended for learning purposes or as a starting point for more complex configurations. Always review and customize infrastructure code to meet the specific needs and security requirements of your environment before deploying to production.

**Feel free to contribute or open issues for suggestions and improvements!**