# Terraform Summary

### What is Terraform?
- Terraform is an open-source **Infrastructure as Code (IaC)** tool by HashiCorp.
- It allows you to **provision and manage infrastructure** using a declarative configuration language.
- It supports **multi-cloud** (AWS, Azure, GCP, etc.).

### Key Features:
- **Declarative** syntax (you describe what you want, not how to do it)
- **State management** (tracks infrastructure state in `.tfstate` files)
- **Immutable infrastructure** (changes create new resources if needed)
- **Provisioning automation** (no need for manual deployment)

### Use Cases:
- Managing cloud resources
- Deploying multi-cloud infrastructure
- Automating infrastructure workflows

---


## Terraform Basics

### Configuration Files:
- **main.tf** – where you define resources
- **variables.tf** – declare input variables
- **outputs.tf** – declare output values
- **terraform.tfvars** – assign values to variables

### Basic Commands:
- `terraform init` – Initialize project and download providers
- `terraform plan` – Show what will happen before changes
- `terraform apply` – Apply the changes
- `terraform destroy` – Remove all managed resources

### File Extension:
- Uses `.tf` (Terraform configuration language)
- `.tf.json` is also supported (JSON format)

---

## Providers

### What is a Provider?
- A **plugin** that lets Terraform interact with APIs (like AWS, Azure, etc.).
- Providers must be **configured** before use.

### Example:
```hcl
provider "aws" {
  region = "us-east-1"
}
```

### Installing Providers:
- Automatically installed via `terraform init`
- Managed in the **Terraform Registry**

### Specifying Provider Version:
```hcl
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
```
}

## Terraform State

### What is Terraform State?
- A file (`terraform.tfstate`) that stores the **current state** of your infrastructure.
- It maps **real-world resources** to your Terraform config.

### Why it's important:
- Enables Terraform to know **what it manages**.
- Used to **plan changes** and **track drift**.

### Key Commands:
- `terraform show`: View current state.
- `terraform state list`: List tracked resources.
- `terraform state show <resource>`: Details of a specific resource.

### Remote State:
- Store state in a **remote backend** like S3 for team collaboration.
- Use with **state locking** to avoid conflicts.

---

## Variables

### Purpose:
Variables make your Terraform config **dynamic and reusable**.

### Types:
- **Input Variables** – Accept values from users.
- **Output Values** – Show useful information after apply.
- **Environment Variables** – Set values from OS environment.

### Input Example:

```hcl
variable "region" {
  type    = string
  default = "us-east-1"
}

provider "aws" {
  region = var.region
}
```
