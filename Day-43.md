
# Git Commands in Bash


## Basic Git Commands

1. Initializing a Repository
```bash
git init
```

2. Cloning a Repository
```bash
git clone <repository_url>
```

3. Checking Status
```bash
git status
```

4. Adding Files
```bash
git add <file>
git add .       # Add all files
```

5. Committing Changes
```bash
git commit -m "<message>"
```

6. Pushing Changes
```bash
git push origin <branch>
```

7. Pulling Changes
```bash
git pull origin <branch>
```

---

## Branch Management

### Creating a New Branch
```bash
git branch <branch_name>
```

### Switching Branches
```bash
git checkout <branch_name>
```

### Creating and Switching to a New Branch
```bash
git checkout -b <new_branch>
```

### Merging Branches
```bash
git merge <branch_name>
```


## Viewing Information

### Viewing Commit History
```bash
git log
git log --oneline   # Compact view
```

### Viewing Changes
```bash
git diff
```

## Examples

### Automating Git Workflow
```bash
#!/bin/bash

# Add all changes
git add .

# Prompt for commit message
read -p "Enter commit message: " message"

# Commit changes
git commit -m "$message"

# Push to remote
git push origin master
```

### Creating a Release Branch
```bash
#!/bin/bash

# Ensure we're on the main branch
git checkout main

# Pull latest changes
git pull origin main

# Create a new release branch
release_branch="release-$(date +%Y%m%d)"
git checkout -b $release_branch

echo "Created new release branch: $release_branch"
```


