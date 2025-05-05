# Terraform

Terraform is an IaC tool that can connect to multiple platforms via providers.

## Install Terraform
```bash
brew tap hashicorp/tap
brew install hashicorp/tap/terraform
```

Infrastructure as Code: Terraform is a provision tool, deploy immutable infrastructure resources. Servers, databases, network components.

Advantages of Terraform is that it can deploy infrastructure accross multiple platforms.

How does it manage infrastructure? Through Providers. API.


## Terraform Summary
Terraform is an open-source Infrastructure as Code (IaC) tool by HashiCorp that allows you to build, manage, and destroy infrastructure efficiently across various platforms using a single binary.

## Key Benefits:
- Cross-platform: Works with cloud (AWS, GCP, Azure), on-prem (vSphere), and 3rd-party APIs.

- Stateful: Tracks changes and manage infrastructure easily

- Version Controlled: All changes are visible and easy to push and pull.

- Consistency: Ensures environments stay consistent across systems.

- Declarative Language (HCL): Easy-to-read .tf files define desired infrastructure states.

- Automation: Automatically figures out what to add, change, or delete.

## Providers:

- Plugins that let Terraform manage resources via APIs.

Not limited to cloud—can manage:

- Networking: Cloudflare, Palo Alto, Infoblox

- Monitoring: Datadog, Grafana, Sumo Logic

- Databases: MongoDB, MySQL, PostgreSQL

- Version Control: GitHub, GitLab, Bitbucket


## Terraform Workflow:
**Init**: Set up project and download providers.

**Plan**: Show what changes will happen.

**Apply**: Apply changes to reach the desired state.

Code Example:
```hcl
resource "aws_instance" "webserver" {
  ami           = "ami-0edab43b6fa892279"
  instance_type = "t2.micro"
}


resource "aws_s3_bucket" "finance" {
  bucket = "finance-21092020"
  tags = {
    Description = "Finance and Payroll"
  }
}


resource "aws_iam_user" "admin-user" {
  name = "lucy"
  tags = {
    Description = "Team Leader"
  }
}
# This code sets up an EC2 instance, an S3 bucket, and a user in AWS.
```


