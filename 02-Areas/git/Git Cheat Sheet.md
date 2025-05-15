

## Configuration

```
# Set user information
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Check configuration
git config --list
```

## Basic Commands

```
# Initialize a repository
git init

# Clone a repository
git clone <repository_url>

# Check repository status
git status

# Add files to staging area
git add <file>
git add .  # Add all files

# Commit changes
git commit -m "Commit message"
```

## Branch Management

```
# List all branches
git branch

# Create a new branch
git branch <branch_name>

# Switch to a branch
git checkout <branch_name>
git switch <branch_name>

# Create and switch to a new branch
git checkout -b <branch_name>
git switch -c <branch_name>
```

## Merging and Rebasing

```
# Merge branches
git checkout main
git merge <branch_name>

# Rebase a branch
git checkout <branch_name>
git rebase main
```

## Remote Repositories

```
# Add a remote repository
git remote add origin <repository_url>

# View remote repositories
git remote -v

# Push changes
git push origin <branch_name>

# Pull changes
git pull origin <branch_name>
```

## Undo Changes

```
# Unstage a file
git reset <file>

# Revert a commit
git revert <commit_hash>

# Reset to a previous commit
git reset --hard <commit_hash>
```

## Stashing Changes

```
# Stash changes
git stash

# Apply stashed changes
git stash apply

# List stashes
git stash list
```

## Logs and History

```
# View commit history
git log

# View simplified history
git log --oneline --graph --all
```

## Tags

```
# Create a tag
git tag <tag_name>

# List tags
git tag
```

## Submodules

```
# Add a submodule
git submodule add <repository_url> <path>

# Update submodules
git submodule update --init --recursive
```

## Advanced Operations

```
# Show differences
git diff

# Interactive rebase
git rebase -i <commit_hash>

# Cherry-pick a commit
git cherry-pick <commit_hash>
```

## Fixing Merge Conflicts

```
# Check conflicting files
git status

# Open conflicting files and resolve manually
# Look for conflict markers (<<<<<<<, =======, >>>>>>>) and edit accordingly

# Mark conflicts as resolved
git add <resolved_file>

# Continue merging or rebasing
git commit -m "Resolved merge conflict"
# If rebasing:
git rebase --continue

# Abort merge or rebase if needed
git merge --abort
git rebase --abort
```