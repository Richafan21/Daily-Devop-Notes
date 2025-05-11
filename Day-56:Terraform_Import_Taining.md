# Terraform Taint, Debugging, and Import - KodeKloud Notes

---

## Terraform Taint

### What it does:
Marks a resource as **tainted**, forcing it to be **destroyed and recreated** during the next `terraform apply`.

### Syntax:

```bash
terraform taint <resource_type.resource_name>
```

### Example:

```bash
terraform taint aws_instance.demo_server
```

This tells Terraform: “Recreate this resource on the next apply.”

### Use Case:
- If a resource is **corrupted**, out of sync, or has an issue not fixable with a regular plan/apply.

### Undo Taint:

```bash
terraform untaint <resource_type.resource_name>
```

By leveraging the taint and untaint commands, you can manage resource lifecycle events efficiently, ensuring that your infrastructure remains consistent with your desired state.

---

##  Terraform Debugging

### Enable Detailed Logs:
Use the `TF_LOG` environment variable to get **debug output**.

| Level      | Description                             |
|------------|-----------------------------------------|
| `TRACE`    | Most detailed logs (internal details)   |
| `DEBUG`    | Useful for debugging issues             |
| `INFO`     | Normal operations info (default)        |
| `WARN`     | Warnings                                |
| `ERROR`    | Errors only                             |

### Usage:

```bash
export TF_LOG=DEBUG
terraform apply
```

Logs are printed to the terminal.

### Save logs to a file:

```bash
export TF_LOG=DEBUG
export TF_LOG_PATH=./terraform-debug.log
terraform apply
```

Then inspect `terraform-debug.log`.

### Disable Logging:

```bash
unset TF_LOG
unset TF_LOG_PATH
```

---

## `terraform import`

### What it does:
Brings **existing infrastructure** (created manually or outside Terraform) into your **Terraform state** file.

###  Syntax:

```bash
terraform import <resource_type.resource_name> <resource_id>
```

### Example:

```bash
terraform import aws_instance.demo_server i-0a49e364a0db8d94d
```

### Important:
- Only **updates the state**, not the `.tf` files.
- You must **write the correct resource block manually** in your `.tf` file **before** importing.

###  Workflow:
1. Write the resource block.
2. Run `terraform import`.
3. Run `terraform plan` to confirm state matches code.
