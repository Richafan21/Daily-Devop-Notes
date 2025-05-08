| Command                                | Description                                                                                   |
|----------------------------------------|-----------------------------------------------------------------------------------------------|
| `terraform state list`                 | Lists all resources in the Terraform state.                                                   |
| `terraform state show <resource>`      | Shows detailed information about a specific resource in the state.                            |
| `terraform state pull`                 | Pulls the latest state from the backend.                                                      |
| `terraform state push <path>`          | Pushes a state file to the backend.                                                           |
| `terraform state rm <resource>`        | Removes a resource from the state.                                                            |
| `terraform state mv <source> <dest>`   | Moves a resource from one state to another (useful for renaming or reassigning resources).    |
| `terraform state replace-provider`     | Replaces provider configurations in the state file.                                           |
| `terraform state untaint <resource>`   | Marks a resource as untainted, removing the need to recreate it on the next apply.            |
| `terraform state taint <resource>`     | Marks a resource as tainted, forcing it to be recreated on the next apply.                    |
| `terraform state list -state=<path>`   | List resources from a specific state file location.                                           |
| `terraform state show -state=<path>`   | Shows detailed information for a resource from a specific state file location.               |
