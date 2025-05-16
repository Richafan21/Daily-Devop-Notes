## Modules

### What are Modules?
Modules are containers for multiple resources that are used together. They allow you to group and reuse configuration.

### Types:
- **Root Module**: The main configuration in your Terraform directory.
- **Child Module**: A module called by another module.
- **Module Registry**: You can use public modules from Terraform Registry.

### Why Use Modules?
- Reusability
- Maintainability
- Organization of code

### Using a Module Example:
```hcl
module "vpc" {
  source = "terraform-aws-modules/vpc/aws"
  version = "3.5.0"
  name = "my-vpc"
  cidr = "10.0.0.0/16"
}
```

### Best Practices:
- Use versioned modules
- Keep modules small and focused
- Use input/output variables

---

## Terraform Cloud and Enterprise

### Terraform Cloud
A SaaS platform to manage Terraform workflows.

### Features:
- Remote state storage
- Version control integration
- Collaboration & access control
- Policy as Code with Sentinel
- Runs infrastructure plans in Terraform Cloud

### Workspaces:
Isolated working environments, each with its own state.

### VCS Integration:
Link to GitHub, GitLab, etc., to auto-trigger Terraform runs on commits.

### CLI-driven Workflows:
You can trigger remote Terraform runs using the CLI with:
```
terraform login
terraform init
terraform plan
terraform apply
```
### Terraform Enterprise
A self-hosted distribution of Terraform Cloud with additional enterprise features.

### Key Benefits:
- Secure state management
- Audit logging
- Private module registry
- SAML/SSO, private networking, policy controls
