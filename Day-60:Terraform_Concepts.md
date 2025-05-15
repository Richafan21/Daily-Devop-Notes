
# Terraform Read, Generate, and Modify Configuration.

---

### `count`
- Used to create **multiple instances** of a resource.
- Indexable using `count.index`.

```hcl
resource "aws_instance" "web" {
  count         = 3
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  tags = {
    Name = "Web-${count.index}"
  }
}
```

## `for_each`
- Used for more **explicit mapping** based on a map or set of strings.

```hcl
resource "aws_instance" "web" {
  for_each = toset(["a", "b", "c"])
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  tags = {
    Name = "Web-${each.key}"
  }
}
```
---

## Provisioners

Used for executing scripts or commands after a resource is created.

### Types:
- `local-exec`: Runs on the machine running Terraform.
- `remote-exec`: Runs on the resource (like an EC2 instance).

```hcl
resource "null_resource" "example" {
  provisioner "local-exec" {
    command = "echo Hello from local-exec"
  }
}
```

Use **sparingly**, as provisioners are not ideal for configuration management.

---


## Local Values

Used to simplify expressions and improve reuse.

```hcl
locals {
  region = "us-east-1"
  instance_type = "t2.micro"
}

resource "aws_instance" "example" {
  ami           = "ami-123456"
  instance_type = local.instance_type
  region        = local.region
}
```

---

## Dynamic Blocks

Used to generate nested blocks dynamically.

```hcl
resource "aws_security_group" "example" {
  name = "example"

  dynamic "ingress" {
    for_each = var.rules
    content {
      from_port   = ingress.value.from_port
      to_port     = ingress.value.to_port
      protocol    = ingress.value.protocol
      cidr_blocks = ingress.value.cidr_blocks
    }
  }
}
```

---

## Splat Expressions

Used to extract values from lists of resources.

### Example:

```hcl
output "instance_ids" {
  value = aws_instance.web[*].id
}
```
