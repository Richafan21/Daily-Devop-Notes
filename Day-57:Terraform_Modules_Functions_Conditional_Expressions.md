# Terraform Modules, Functions, and Conditional Expressions
---

## Terraform Modules

### What is a Module?
A **module** is a container for multiple resources that are used together. Every Terraform configuration has at least one module (the root module).

### Benefits:
- Reusability
- Maintainability
- Abstraction

### Using a Module:
You can create a directory with its own `.tf` files and use it in a root configuration.

### Structure:

```yaml
root/
├── main.tf # Calls the module
└── modules/
└── mymodule/
├── main.tf
├── variables.tf
└── outputs.tf
```

### Example Usage:
```hcl
module "my_ec2" {
  source = "./modules/ec2"
  instance_type = "t2.micro"
  ami_id = "ami-123456"
}
```

---


## Terraform Functions

Terraform comes with built-in **functions** for string manipulation, numeric operations, encoding/decoding, etc.

### Examples:

#### String Functions:
```hcl
lower("HELLO")       => "hello"
upper("hello")       => "HELLO"
trim("  hi  ")       => "hi"
replace("a-b-c", "-", ":") => "a:b:c"
```

#### Numeric Functions:
```hcl
max(5, 10, 15)        => 15
min(5, 10, 15)        => 5
abs(-3)               => 3
```

#### File Functions:
```hcl
file("user_data.sh")   # Loads file content
```

---


## Conditional Expressions

Used for decision-making logic (like an if-else).

### Syntax:
```hcl
condition ? true_value : false_value
```

### Example:
```hcl
var.env == "prod" ? "t3.large" : "t3.micro"
```

This assigns `t3.large` if `var.env` is `"prod"`, else `t3.micro`.

