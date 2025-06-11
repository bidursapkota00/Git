# Complete Git Course Guide

## Table of Contents

1. [Course Orientation](#course-orientation)
2. [Introducing Git](#introducing-git)
3. [Installation & Setup](#installation--setup)
4. [The Very Basics Of Git: Adding & Committing](#the-very-basics-of-git-adding--committing)
5. [Commits In Detail (And Related Topics)](#commits-in-detail-and-related-topics)
6. [Working With Branches](#working-with-branches)
7. [Merging Branches, Oh Boy!](#merging-branches-oh-boy)
8. [Comparing Changes With Git Diff](#comparing-changes-with-git-diff)
9. [The Ins and Outs of Stashing](#the-ins-and-outs-of-stashing)
10. [Undoing Changes & Time Traveling](#undoing-changes--time-traveling)
11. [GitHub: The Basics](#github-the-basics)
12. [Fetching & Pulling](#fetching--pulling)
13. [GitHub Grab Bag: Odds & Ends](#github-grab-bag-odds--ends)
14. [Git Collaboration Workflows](#git-collaboration-workflows)
15. [Rebasing: The Scariest Git Command?](#rebasing-the-scariest-git-command)
16. [Cleaning Up History With Interactive Rebase](#cleaning-up-history-with-interactive-rebase)
17. [Git Tags: Marking Important Moments In History](#git-tags-marking-important-moments-in-history)
18. [Git Behind The Scenes - Hashing & Objects](#git-behind-the-scenes---hashing--objects)
19. [The Power of Reflogs - Retrieving "Lost" Work](#the-power-of-reflogs---retrieving-lost-work)
20. [Writing Custom Git Aliases](#writing-custom-git-aliases)

---

## Course Orientation

Welcome to the comprehensive Git course! This guide will take you from a complete beginner to an advanced Git user. Git is the most popular version control system used by developers worldwide, and mastering it is essential for modern software development.

### What You'll Learn

- Complete Git workflow from initialization to collaboration
- Advanced Git features and best practices
- GitHub integration and remote repository management
- Problem-solving techniques for common Git scenarios
- Professional workflows used in real-world projects

---

## Introducing Git

### What is Git?

Git is a distributed version control system that tracks changes in files and coordinates work among multiple people. It was created by Linus Torvalds in 2005 for Linux kernel development.

### Key Concepts

- **Repository (Repo)**: A directory containing your project files and Git's tracking information
- **Version Control**: System that records changes to files over time
- **Distributed**: Every user has a complete copy of the project history
- **Snapshot**: Git stores data as snapshots of your project at different points in time

### Why Use Git?

- Track changes and history of your project
- Collaborate with multiple developers
- Create branches for different features
- Revert to previous versions when needed
- Backup your code in multiple locations

---

## Installation & Setup

### Installing Git

#### Windows

1. Download Git from [git-scm.com](https://git-scm.com)
2. Run the installer with default settings
3. Open Git Bash or Command Prompt

#### macOS

```bash
# Using Homebrew
brew install git

# Or download from git-scm.com
```

#### Linux (Ubuntu/Debian)

```bash
sudo apt update
sudo apt install git
```

### Initial Configuration

```bash
# Set your name and email (required for commits)
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Set default branch name
git config --global init.defaultBranch main

# Set default editor
git config --global core.editor "code --wait"  # For VS Code

# Check your configuration
git config --list
```

### Verify Installation

```bash
git --version
```

---

## The Very Basics Of Git: Adding & Committing

### Initializing a Repository

```bash
# Create a new directory
mkdir my-project
cd my-project

# Initialize Git repository
git init
```

### The Three Areas

1. **Working Directory**: Your actual files
2. **Staging Area**: Files prepared for commit
3. **Repository**: Committed snapshots

### Basic Workflow

```bash
# Check repository status
git status

# Add files to staging area
git add filename.txt          # Add specific file
git add .                     # Add all files
git add *.js                  # Add all JavaScript files

# Commit changes
git commit -m "Initial commit"

# View commit history
git log
git log --oneline            # Condensed view
```

### Example Workflow

```bash
# Create a file
echo "Hello Git!" > hello.txt

# Check status
git status
# Output: Untracked files: hello.txt

# Add to staging
git add hello.txt

# Check status again
git status
# Output: Changes to be committed: new file: hello.txt

# Commit
git commit -m "Add hello.txt file"

# View history
git log --oneline
```

---

## Commits In Detail (And Related Topics)

### Anatomy of a Commit

Each commit contains:

- **SHA-1 Hash**: Unique identifier (e.g., `a1b2c3d4...`)
- **Author**: Name and email
- **Date**: When the commit was made
- **Message**: Description of changes
- **Parent**: Reference to previous commit(s)

### Writing Good Commit Messages

```bash
# Good commit messages
git commit -m "Add user authentication feature"
git commit -m "Fix memory leak in image processing"
git commit -m "Update README with installation instructions"

# Bad commit messages
git commit -m "stuff"
git commit -m "fix"
git commit -m "changes"
```

### Commit Message Conventions

```bash
# Structure: <type>: <description>
git commit -m "feat: add login functionality"
git commit -m "fix: resolve null pointer exception"
git commit -m "docs: update API documentation"
git commit -m "style: format code with prettier"
git commit -m "refactor: extract utility functions"
```

### Amending Commits

```bash
# Fix the last commit message
git commit --amend -m "New commit message"

# Add forgotten files to last commit
git add forgotten-file.txt
git commit --amend --no-edit
```

### Viewing Commit Details

```bash
# Show commit details
git show
git show <commit-hash>

# Show files changed in commit
git show --name-only <commit-hash>

# Show statistics
git show --stat <commit-hash>
```

---

## Working With Branches

### What are Branches?

Branches allow you to work on different features or experiments without affecting the main codebase. Think of them as parallel universes of your project.

### Branch Commands

```bash
# List all branches
git branch

# Create new branch
git branch feature-login

# Switch to branch
git checkout feature-login
# Or use newer syntax
git switch feature-login

# Create and switch in one command
git checkout -b feature-signup
# Or
git switch -c feature-signup

# Delete branch
git branch -d feature-login     # Safe delete
git branch -D feature-login     # Force delete
```

### Branch Workflow Example

```bash
# Start on main branch
git checkout main

# Create feature branch
git checkout -b add-shopping-cart

# Make changes
echo "Shopping cart functionality" > cart.js
git add cart.js
git commit -m "Add shopping cart feature"

# Switch back to main
git checkout main

# The cart.js file won't be visible here!
```

### Renaming Branches

```bash
# Rename current branch
git branch -m new-branch-name

# Rename specific branch
git branch -m old-name new-name
```

---

## Merging Branches, Oh Boy!

### Types of Merges

#### 1. Fast-Forward Merge

When the target branch hasn't changed since branching:

```bash
# On main branch
git merge feature-branch
# Results in fast-forward merge
```

#### 2. Three-Way Merge

When both branches have new commits:

```bash
git merge feature-branch
# Creates a merge commit
```

### Merge Example

```bash
# Create and work on feature branch
git checkout -b feature-navbar
echo "Navigation bar" > navbar.js
git add navbar.js
git commit -m "Add navigation bar"

# Switch to main and make different changes
git checkout main
echo "Main content" > main.js
git add main.js
git commit -m "Add main content"

# Merge feature branch
git merge feature-navbar
# This creates a merge commit
```

### Merge Conflicts

When Git can't automatically merge:

```bash
# Conflict example
git merge feature-branch
# Output: CONFLICT (content): Merge conflict in file.txt

# View conflicted files
git status

# Edit conflicted file - look for markers:
<<<<<<< HEAD
Content from current branch
=======
Content from feature branch
>>>>>>> feature-branch

# After resolving conflicts:
git add file.txt
git commit  # No message needed for merge commits
```

### Merge Strategies

```bash
# No fast-forward (always create merge commit)
git merge --no-ff feature-branch

# Squash merge (combine all commits into one)
git merge --squash feature-branch
git commit -m "Add feature XYZ"
```

---

## Comparing Changes With Git Diff

### Basic Diff Commands

```bash
# Show unstaged changes
git diff

# Show staged changes
git diff --staged
# Or
git diff --cached

# Compare specific files
git diff filename.txt

# Compare branches
git diff main feature-branch

# Compare specific commits
git diff commit1 commit2
```

### Diff Output Explanation

```diff
diff --git a/file.txt b/file.txt
index 83db48f..84d55c5 100644
--- a/file.txt
+++ b/file.txt
@@ -1,3 +1,4 @@
 line 1
-line 2
+line 2 modified
+new line 3
 line 4
```

### Advanced Diff Options

```bash
# Word-level diff
git diff --word-diff

# Show only names of changed files
git diff --name-only

# Show statistics
git diff --stat

# Ignore whitespace changes
git diff --ignore-space-change
```

---

## The Ins and Outs of Stashing

### What is Stashing?

Stashing temporarily saves your uncommitted changes so you can work on something else and then come back and re-apply them.

### Basic Stashing

```bash
# Stash current changes
git stash

# List all stashes
git stash list

# Apply most recent stash
git stash pop

# Apply stash without removing it
git stash apply

# Drop a stash
git stash drop
```

### Advanced Stashing

```bash
# Stash with message
git stash push -m "Work in progress on user login"

# Stash specific files
git stash push -m "Partial work" file1.txt file2.txt

# Stash including untracked files
git stash -u

# Apply specific stash
git stash apply stash@{2}

# Show stash contents
git stash show -p stash@{0}
```

### Stashing Workflow Example

```bash
# Working on feature
echo "Incomplete feature" > feature.js
git add feature.js

# Urgent bug fix needed
git stash push -m "Incomplete login feature"

# Switch to main and fix bug
git checkout main
echo "Bug fix" > bugfix.js
git add bugfix.js
git commit -m "Fix critical bug"

# Return to feature work
git checkout feature-branch
git stash pop
# Continue working on feature.js
```

---

## Undoing Changes & Time Traveling

### Undoing Working Directory Changes

```bash
# Discard changes in working directory
git checkout -- filename.txt
# Or newer syntax
git restore filename.txt

# Discard all working directory changes
git checkout -- .
git restore .
```

### Undoing Staged Changes

```bash
# Unstage file
git reset HEAD filename.txt
# Or newer syntax
git restore --staged filename.txt

# Unstage all files
git reset HEAD
```

### Undoing Commits

#### 1. Reset (Changes History)

```bash
# Soft reset - keep changes in staging
git reset --soft HEAD~1

# Mixed reset - keep changes in working directory
git reset --mixed HEAD~1
git reset HEAD~1  # Default is mixed

# Hard reset - discard all changes
git reset --hard HEAD~1
```

#### 2. Revert (Safe for Shared Repos)

```bash
# Create new commit that undoes previous commit
git revert HEAD

# Revert specific commit
git revert <commit-hash>

# Revert multiple commits
git revert HEAD~3..HEAD
```

### Reset vs Revert Example

```bash
# Scenario: Need to undo last commit

# Using reset (dangerous if already pushed)
git reset --hard HEAD~1

# Using revert (safe for shared repos)
git revert HEAD
```

---

## GitHub: The Basics

### What is GitHub?

GitHub is a cloud-based Git repository hosting service that adds collaboration features, issue tracking, and project management tools.

### Setting Up GitHub

#### SSH Keys (Recommended)

```bash
# Generate SSH key
ssh-keygen -t ed25519 -C "your.email@example.com"

# Add to SSH agent
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519

# Copy public key to clipboard
cat ~/.ssh/id_ed25519.pub
# Add this to GitHub Settings > SSH Keys
```

#### HTTPS with Personal Access Token

1. Go to GitHub Settings > Developer settings > Personal access tokens
2. Generate new token with appropriate permissions
3. Use token as password when prompted

### Repository Operations

```bash
# Clone repository
git clone https://github.com/username/repo-name.git
git clone git@github.com:username/repo-name.git  # SSH

# Add remote origin
git remote add origin https://github.com/username/repo-name.git

# Check remotes
git remote -v

# Push to GitHub
git push origin main

# Set upstream for easier pushing
git push -u origin main
git push  # Now this works
```

### GitHub Workflow

```bash
# 1. Clone or create repo
git clone https://github.com/username/project.git
cd project

# 2. Create feature branch
git checkout -b feature-new-header

# 3. Make changes and commit
echo "New header" > header.html
git add header.html
git commit -m "Add new header component"

# 4. Push branch to GitHub
git push origin feature-new-header

# 5. Create Pull Request on GitHub
# 6. Merge after review
# 7. Delete branch after merge
git branch -d feature-new-header
```

---

## Fetching & Pulling

### Understanding Remote Tracking

```bash
# View remote branches
git branch -r

# View all branches (local and remote)
git branch -a

# View remote information
git remote show origin
```

### Fetch vs Pull

#### Fetch (Download without Merging)

```bash
# Fetch all branches from all remotes
git fetch

# Fetch specific remote
git fetch origin

# Fetch specific branch
git fetch origin main
```

#### Pull (Fetch + Merge)

```bash
# Pull current branch
git pull

# Pull specific branch
git pull origin main

# Pull with rebase instead of merge
git pull --rebase
```

### Collaboration Workflow

```bash
# Start of day - get latest changes
git checkout main
git pull origin main

# Create feature branch from updated main
git checkout -b feature-user-profile

# Work on feature...
git add .
git commit -m "Add user profile page"

# Before pushing, get latest changes
git checkout main
git pull origin main

# Merge or rebase feature branch
git checkout feature-user-profile
git rebase main  # Or merge main into feature branch

# Push feature branch
git push origin feature-user-profile
```

### Handling Merge Conflicts During Pull

```bash
# Pull results in conflict
git pull origin main
# Fix conflicts in files
git add conflicted-file.txt
git commit
```

---

## GitHub Grab Bag: Odds & Ends

### Forking Workflow

```bash
# 1. Fork repository on GitHub
# 2. Clone your fork
git clone https://github.com/yourusername/forked-repo.git

# 3. Add upstream remote
git remote add upstream https://github.com/original-owner/repo.git

# 4. Keep fork updated
git fetch upstream
git checkout main
git merge upstream/main
git push origin main
```

### Pull Requests

- Create descriptive titles and descriptions
- Reference issues with `#issue-number`
- Request specific reviewers
- Use draft PRs for work in progress
- Link to related issues or documentation

### GitHub Issues

```markdown
## Bug Report

**Description:** Brief description of the bug
**Steps to Reproduce:**

1. Go to page X
2. Click button Y
3. Error occurs

**Expected Behavior:** What should happen
**Actual Behavior:** What actually happens
**Environment:** Browser, OS, version
```

### GitHub Pages

```bash
# Deploy to GitHub Pages
# 1. Create gh-pages branch
git checkout -b gh-pages

# 2. Add HTML files
echo "<h1>My Project</h1>" > index.html
git add index.html
git commit -m "Add GitHub Pages site"

# 3. Push to GitHub
git push origin gh-pages

# Site available at: https://username.github.io/repository-name
```

### README Best Practices

````markdown
# Project Title

Brief description of what this project does

## Installation

```bash
npm install
```
````

## Usage

```bash
npm start
```

## Contributing

1. Fork the project
2. Create feature branch
3. Commit changes
4. Push to branch
5. Open Pull Request

## License

MIT License

````

---

## Git Collaboration Workflows

### Centralized Workflow
Simple workflow for small teams:
```bash
# Everyone works on main branch
git clone repo
# Make changes
git add .
git commit -m "Add feature"
git pull  # Get latest changes
git push  # Push your changes
````

### Feature Branch Workflow

Most common workflow:

```bash
# 1. Create feature branch
git checkout -b feature-user-auth

# 2. Work on feature
git add .
git commit -m "Implement user authentication"

# 3. Push branch
git push origin feature-user-auth

# 4. Create Pull Request
# 5. Review and merge
# 6. Delete branch
git branch -d feature-user-auth
```

### Gitflow Workflow

More structured workflow for larger projects:

```bash
# Main branches: main (production) and develop

# Feature branches
git checkout develop
git checkout -b feature/user-login
# Work on feature
git checkout develop
git merge feature/user-login

# Release branches
git checkout -b release/1.2.0 develop
# Bug fixes only
git checkout main
git merge release/1.2.0
git checkout develop
git merge release/1.2.0

# Hotfix branches
git checkout -b hotfix/critical-bug main
# Fix bug
git checkout main
git merge hotfix/critical-bug
git checkout develop
git merge hotfix/critical-bug
```

### Forking Workflow

For open source projects:

```bash
# 1. Fork repository on GitHub
# 2. Clone fork
git clone https://github.com/yourfork/project.git

# 3. Add upstream
git remote add upstream https://github.com/original/project.git

# 4. Create feature branch
git checkout -b new-feature

# 5. Make changes and push to fork
git push origin new-feature

# 6. Create Pull Request from fork to original repo
```

---

## Rebasing: The Scariest Git Command?

### What is Rebasing?

Rebasing re-applies commits from one branch onto another, creating a linear history.

### Merge vs Rebase

```bash
# Merge creates a merge commit
git checkout main
git merge feature-branch

# Rebase creates linear history
git checkout feature-branch
git rebase main
git checkout main
git merge feature-branch  # Fast-forward merge
```

### Basic Rebase

```bash
# Rebase current branch onto main
git rebase main

# Rebase specific branch
git rebase main feature-branch
```

### Interactive Rebase

```bash
# Rebase last 3 commits interactively
git rebase -i HEAD~3

# Interactive rebase options:
# pick - use commit as is
# reword - change commit message
# edit - modify commit
# squash - combine with previous commit
# drop - remove commit
```

### Rebase Example

```bash
# Before rebase:
# main:    A---B---C
# feature:     D---E---F

git checkout feature
git rebase main

# After rebase:
# main:    A---B---C
# feature:         D'---E'---F'
```

### Golden Rule of Rebasing

**Never rebase commits that have been pushed to a shared repository!**

### Handling Rebase Conflicts

```bash
# Conflict during rebase
git rebase main
# Fix conflicts
git add conflicted-file.txt
git rebase --continue

# Skip problematic commit
git rebase --skip

# Abort rebase
git rebase --abort
```

---

## Cleaning Up History With Interactive Rebase

### Interactive Rebase Commands

```bash
# Start interactive rebase
git rebase -i HEAD~4

# Example interactive rebase screen:
pick 1234567 Add login feature
reword 2345678 Fix typo in login
squash 3456789 Update login styling
drop 4567890 Debug logging
```

### Common Interactive Rebase Tasks

#### 1. Squashing Commits

```bash
git rebase -i HEAD~3

# Change commits to:
pick 1234567 Add user authentication
squash 2345678 Fix authentication bug
squash 3456789 Add authentication tests

# Result: One commit with all changes
```

#### 2. Reordering Commits

```bash
# Original order:
pick A Add feature X
pick B Add feature Y
pick C Fix feature X

# Reorder to:
pick A Add feature X
pick C Fix feature X
pick B Add feature Y
```

#### 3. Editing Commits

```bash
git rebase -i HEAD~2

# Change to:
edit 1234567 Add user profile
pick 2345678 Update navigation

# Git stops at the edit commit
# Make changes
git add modified-file.txt
git commit --amend
git rebase --continue
```

#### 4. Splitting Commits

```bash
git rebase -i HEAD~1

# Change to:
edit 1234567 Large commit to split

# When Git stops:
git reset HEAD~1  # Unstage changes
git add file1.txt
git commit -m "Add file1 functionality"
git add file2.txt
git commit -m "Add file2 functionality"
git rebase --continue
```

---

## Git Tags: Marking Important Moments In History

### What are Tags?

Tags mark specific points in Git history, typically used for releases.

### Types of Tags

#### 1. Lightweight Tags

```bash
# Create lightweight tag
git tag v1.0

# Tag specific commit
git tag v1.0 <commit-hash>
```

#### 2. Annotated Tags (Recommended)

```bash
# Create annotated tag
git tag -a v1.0 -m "Release version 1.0"

# Tag with detailed message
git tag -a v1.0 -m "
Version 1.0 Release

Features:
- User authentication
- Shopping cart
- Payment processing
"
```

### Working with Tags

```bash
# List all tags
git tag

# List tags matching pattern
git tag -l "v1.*"

# Show tag information
git show v1.0

# Checkout specific tag
git checkout v1.0

# Delete tag
git tag -d v1.0
```

### Pushing Tags

```bash
# Push specific tag
git push origin v1.0

# Push all tags
git push origin --tags

# Push annotated tags only
git push --follow-tags
```

### Semantic Versioning with Tags

```bash
# Major.Minor.Patch
git tag -a v1.0.0 -m "Initial release"
git tag -a v1.0.1 -m "Bug fix release"
git tag -a v1.1.0 -m "Minor feature release"
git tag -a v2.0.0 -m "Major release with breaking changes"
```

### Release Workflow

```bash
# 1. Finish features for release
git checkout main
git pull origin main

# 2. Update version numbers in code
# Edit package.json, version files, etc.
git add .
git commit -m "Bump version to 1.2.0"

# 3. Create release tag
git tag -a v1.2.0 -m "Release version 1.2.0"

# 4. Push changes and tags
git push origin main
git push origin v1.2.0

# 5. Create GitHub release from tag
```

---

## Git Behind The Scenes - Hashing & Objects

### Git Object Types

#### 1. Blob Objects (File Contents)

```bash
# View blob object
git show <blob-hash>

# Create blob manually
echo "Hello World" | git hash-object -w --stdin
```

#### 2. Tree Objects (Directory Structure)

```bash
# View tree object
git ls-tree <tree-hash>

# Show current tree
git ls-tree HEAD
```

#### 3. Commit Objects

```bash
# View commit object
git cat-file -p <commit-hash>

# Example output:
tree 4b825dc642cb6eb9a060e54bf8d69288fbee4904
author John Doe <john@example.com> 1234567890 +0000
committer John Doe <john@example.com> 1234567890 +0000

Initial commit
```

### SHA-1 Hashing

```bash
# Generate hash for content
echo "Hello Git" | git hash-object --stdin

# Every object has unique 40-character SHA-1 hash
# First few characters usually sufficient for reference
git show 1a2b3c  # Instead of full hash
```

### Git Directory Structure

```
.git/
├── objects/        # All Git objects (blobs, trees, commits)
├── refs/          # Branch and tag references
│   ├── heads/     # Branch references
│   └── tags/      # Tag references
├── HEAD           # Current branch reference
├── index          # Staging area
└── config         # Repository configuration
```

### Exploring Git Internals

```bash
# View object type
git cat-file -t <hash>

# View object content
git cat-file -p <hash>

# View object size
git cat-file -s <hash>

# View current HEAD
cat .git/HEAD

# View branch reference
cat .git/refs/heads/main
```

---

## The Power of Reflogs - Retrieving "Lost" Work

### What is Reflog?

Reflog (reference log) tracks changes to branch tips and other references, acting as a safety net for "lost" commits.

### Viewing Reflog

```bash
# View reflog for current branch
git reflog

# View reflog for specific branch
git reflog feature-branch

# View reflog for all references
git reflog --all
```

### Reflog Output Example

```
abc1234 HEAD@{0}: commit: Add user authentication
def5678 HEAD@{1}: checkout: moving from main to feature-auth
9012345 HEAD@{2}: commit: Update README
```

### Recovering Lost Commits

```bash
# Scenario: Accidentally reset hard
git reset --hard HEAD~3  # Oops! Lost 3 commits

# Find lost commits in reflog
git reflog
# Output shows: abc1234 HEAD@{1}: commit: Important feature

# Recover the lost commit
git reset --hard abc1234
# Or create new branch from lost commit
git branch recovery-branch abc1234
```

### Recovering Deleted Branches

```bash
# Accidentally delete branch
git branch -D important-feature  # Oops!

# Find branch in reflog
git reflog --all | grep important-feature

# Recreate branch
git branch important-feature <commit-hash>
```

### Reflog Expiration

```bash
# Reflog entries expire after 90 days by default
# Configure expiration
git config gc.reflogExpire "never"
git config gc.reflogExpireUnreachable "never"

# Manually clean reflog
git reflog expire --expire=30.days refs/heads/main
```

### Advanced Reflog Usage

```bash
# View reflog with dates
git reflog --date=iso

# Find when file was deleted
git log --oneline --follow -- deleted-file.txt
git reflog --grep="deleted-file"

# Recovery workflow
git checkout <commit-before-deletion>
git checkout -b recover-file
# File should be present here
```

---

## Writing Custom Git Aliases

### What are Git Aliases?

Aliases are shortcuts for longer Git commands, making your workflow more efficient.

### Setting Up Aliases

```bash
# Basic alias syntax
git config --global alias.<shortcut> '<command>'

# Example aliases
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status
```

### Useful Aliases

#### Status and Log Shortcuts

```bash
# Short status
git config --global alias.s "status -s"

# Pretty log
git config --global alias.lg "log --oneline --graph --decorate --all"

# Detailed log with stats
git config --global alias.ll "log --stat --abbrev-commit"

# Log with graph
git config --global alias.tree "log --graph --pretty=format:'%Cred%h%Creset %an: %s - %Creset %C(yellow)%d%Creset %Cgreen(%cr)%Creset' --abbrev-commit --date=relative"
```

#### Branch Management

```bash
# List branches sorted by last commit
git config --global alias.recent "branch --sort=-committerdate"

# Delete merged branches
git config --global alias.cleanup "branch --merged | grep -v '\\*\\|main\\|develop' | xargs -n 1 git branch -d"

# Current branch name
git config --global alias.current "rev-parse --abbrev-ref HEAD"
```

#### Diff and Show Shortcuts

```bash
# Show diff of staged files
git config --global alias.staged "diff --staged"

# Show diff with word highlighting
git config --global alias.wdiff "diff --word-diff"

# Show files in last commit
git config --global alias.last "show --name-only"
```

#### Commit Shortcuts

```bash
# Quick commit all changes
git config --global alias.ca "commit -a"

# Amend last commit
git config --global alias.amend "commit --amend"

# Commit with message
git config --global alias.cm "commit -m"
```

#### Advanced Aliases

```bash
# Undo last commit (keep changes)
git config --global alias.undo "reset HEAD~1 --mixed"

# Show contributors
git config --global alias.contributors "shortlog --summary --numbered"

# Find commits by message
git config --global alias.find "log --grep"

# Show ignored files
git config --global alias.ignored "ls-files --others --ignored --exclude-standard"
```

### Complex Aliases with Shell Commands

```bash
# Publish current branch to origin
git config --global alias.publish "!git push -u origin $(git branch --show-current)"

# Delete branch locally and remotely
git config --global alias.nuke "!f() { git branch -D $1; git push origin --delete $1; }; f"

# Create branch and switch to it
git config --global alias.nb "!f() { git checkout -b $1; git push -u origin $1; }; f"
```

### Managing Aliases

```bash
# View all aliases
git config --global --get-regexp alias

# Edit Git config file directly
git config --global --edit

# Remove alias
git config --global --unset alias.shortcut
```

### Alias Configuration File

Add to `~/.gitconfig`:

```ini
[alias]
    co = checkout
    br = branch
    ci = commit
    st = status
    s = status -s
    lg = log --oneline --graph --decorate --all
    ll = log --stat --abbrev-commit
    tree = log --graph --pretty=format:'%Cred%h%Creset %an: %s - %Creset %C(yellow)%d%Creset %Cgreen(%cr)%Creset' --abbrev-commit --date=relative
    recent = branch --sort=-committerdate
    cleanup = !git branch --merged | grep -v '\\*\\|main\\|develop' | xargs -n 1 git branch -d
    current = rev-parse --abbrev-ref HEAD
    staged = diff --staged
    wdiff = diff --word-diff
    last = show --name-only
    ca = commit -a
    amend = commit --amend
    cm = commit -m
    undo = reset HEAD~1 --mixed
    contributors = shortlog --summary --numbered
    find = log --grep
    ignored = ls-files --others --ignored --exclude-standard
    publish = !git push -u origin $(git branch --show-current)
    nuke = "!f() { git branch -D $1; git push origin --delete $1; }; f"
    nb = "!f() { git checkout -b $1; git push -u origin $1; }; f"
```

---

## Advanced Git Techniques and Best Practices

### Git Hooks

Git hooks are scripts that run automatically at certain points in the Git workflow.

#### Common Hooks

```bash
# Location: .git/hooks/
# Make executable: chmod +x .git/hooks/hook-name

# pre-commit: Run before commit
#!/bin/sh
# .git/hooks/pre-commit
npm test
if [ $? -ne 0 ]; then
    echo "Tests failed. Commit aborted."
    exit 1
fi

# pre-push: Run before push
#!/bin/sh
# .git/hooks/pre-push
npm run lint
if [ $? -ne 0 ]; then
    echo "Linting failed. Push aborted."
    exit 1
fi

# post-commit: Run after commit
#!/bin/sh
# .git/hooks/post-commit
echo "Commit completed: $(git log -1 --pretty=format:'%h %s')"
```

#### Setting Up Hooks

```bash
# Copy hook template
cp .git/hooks/pre-commit.sample .git/hooks/pre-commit

# Make executable
chmod +x .git/hooks/pre-commit

# Share hooks with team (they go in version control)
mkdir .githooks
# Put hooks in .githooks/
git config core.hooksPath .githooks
```

### Git Worktrees

Work on multiple branches simultaneously without switching.

```bash
# List worktrees
git worktree list

# Create new worktree
git worktree add ../project-feature feature-branch

# Create worktree with new branch
git worktree add ../project-hotfix -b hotfix-urgent

# Remove worktree
git worktree remove ../project-feature

# Prune deleted worktrees
git worktree prune
```

### Git Submodules

Include other Git repositories as subdirectories.

```bash
# Add submodule
git submodule add https://github.com/user/library.git lib/library

# Clone repository with submodules
git clone --recursive https://github.com/user/project.git

# Initialize submodules in existing repo
git submodule init
git submodule update

# Update submodules
git submodule update --remote

# Remove submodule
git submodule deinit lib/library
git rm lib/library
rm -rf .git/modules/lib/library
```

### Advanced Merge Strategies

```bash
# Merge with specific strategy
git merge -X ours feature-branch    # Prefer current branch
git merge -X theirs feature-branch  # Prefer incoming branch

# Octopus merge (multiple branches)
git merge branch1 branch2 branch3

# Subtree merge
git merge -s subtree feature-branch
```

### Git Bisect - Finding Bugs

Binary search to find the commit that introduced a bug.

```bash
# Start bisect
git bisect start

# Mark current commit as bad
git bisect bad

# Mark known good commit
git bisect good v1.0

# Git checks out middle commit
# Test and mark as good or bad
git bisect good    # or git bisect bad

# Continue until Git finds the problematic commit
# Reset when done
git bisect reset
```

### Git Blame and Annotate

Find who changed specific lines of code.

```bash
# Show who changed each line
git blame filename.txt

# Blame specific line range
git blame -L 10,20 filename.txt

# Show original commit for each line
git annotate filename.txt

# Ignore whitespace changes
git blame -w filename.txt
```

### Git Archive

Create archives of your repository.

```bash
# Create zip archive
git archive --format=zip HEAD > project.zip

# Create tar archive
git archive --format=tar HEAD | gzip > project.tar.gz

# Archive specific branch or tag
git archive --format=zip v1.0 > release-1.0.zip

# Archive with prefix
git archive --prefix=project/ HEAD | gzip > project.tar.gz
```

### Performance and Maintenance

```bash
# Garbage collection (cleanup)
git gc

# Aggressive cleanup
git gc --aggressive

# Check repository integrity
git fsck

# Show repository size
git count-objects -vH

# Prune unreachable objects
git prune

# Clean untracked files
git clean -fd    # Force remove files and directories
git clean -n     # Dry run (show what would be removed)
```

---

## Git Configuration Deep Dive

### Configuration Levels

```bash
# System-wide configuration
git config --system user.name "System User"
# Location: /etc/gitconfig

# User-wide configuration
git config --global user.name "Global User"
# Location: ~/.gitconfig

# Repository-specific configuration
git config user.name "Repo User"
# Location: .git/config
```

### Essential Configuration

```bash
# User information
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Default editor
git config --global core.editor "code --wait"

# Default branch name
git config --global init.defaultBranch main

# Merge tool
git config --global merge.tool vimdiff

# Diff tool
git config --global diff.tool vimdiff

# Auto-correct typos
git config --global help.autocorrect 1

# Colorful output
git config --global color.ui auto

# Line ending handling
git config --global core.autocrlf input    # Linux/Mac
git config --global core.autocrlf true     # Windows
```

### Advanced Configuration

```bash
# Push only current branch
git config --global push.default simple

# Automatically setup remote tracking
git config --global push.autoSetupRemote true

# Reuse recorded resolutions
git config --global rerere.enabled true

# Show branch names in prompt
git config --global branch.autosetupmerge always

# Credential helper
git config --global credential.helper cache
git config --global credential.helper 'cache --timeout=3600'

# GPG signing
git config --global user.signingkey YOUR_GPG_KEY
git config --global commit.gpgsign true
```

---

## Git Security and GPG Signing

### Setting Up GPG Signing

```bash
# Generate GPG key
gpg --full-generate-key

# List GPG keys
gpg --list-secret-keys --keyid-format LONG

# Export public key
gpg --armor --export YOUR_KEY_ID

# Configure Git to use GPG
git config --global user.signingkey YOUR_KEY_ID
git config --global commit.gpgsign true
git config --global tag.forcesignannotated true
```

### Signing Commits and Tags

```bash
# Sign specific commit
git commit -S -m "Signed commit"

# Sign tag
git tag -s v1.0 -m "Signed tag"

# Verify signatures
git log --show-signature
git verify-commit HEAD
git verify-tag v1.0
```

### Security Best Practices

```bash
# Use SSH keys for authentication
ssh-keygen -t ed25519 -C "your.email@example.com"

# Use credential manager
git config --global credential.helper manager-core

# Verify repository integrity
git fsck --full

# Check for sensitive data
git log --all --full-history -- path/to/sensitive/file
```

---

## Troubleshooting Common Git Issues

### Repository Corruption

```bash
# Check repository integrity
git fsck --full

# Recover from corruption
git reflog expire --expire-unreachable=now --all
git gc --prune=now

# Clone from backup if necessary
git clone --mirror backup-url recovered-repo
```

### Large File Issues

```bash
# Find large files
git rev-list --objects --all | grep "$(git verify-pack -v .git/objects/pack/*.idx | sort -k 3 -n | tail -10 | awk '{print $1}')"

# Remove large file from history
git filter-branch --force --index-filter 'git rm --cached --ignore-unmatch path/to/large/file' --prune-empty --tag-name-filter cat -- --all

# Alternative: use BFG Repo-Cleaner
java -jar bfg.jar --strip-blobs-bigger-than 100M my-repo.git
```

### Merge Conflicts Resolution

```bash
# Configure merge tool
git config --global merge.tool vimdiff
git config --global mergetool.vimdiff.cmd 'vimdiff $LOCAL $MERGED $REMOTE'

# Use merge tool
git mergetool

# Abort merge
git merge --abort

# Continue after resolving conflicts
git add resolved-file.txt
git commit
```

### Detached HEAD State

```bash
# You're in detached HEAD state
git checkout -b new-branch-name

# Or return to previous branch
git checkout -

# Or checkout specific branch
git checkout main
```

### Accidentally Committed to Wrong Branch

```bash
# Move commits to correct branch
git checkout correct-branch
git cherry-pick commit-hash

# Remove from wrong branch
git checkout wrong-branch
git reset --hard HEAD~1
```

---

## Git Performance Optimization

### Repository Maintenance

```bash
# Regular maintenance
git maintenance start

# Manual maintenance tasks
git gc --aggressive
git repack -Ad
git prune-packed
git reflog expire --expire=now --all
```

### Partial Clone and Sparse Checkout

```bash
# Partial clone (no blobs)
git clone --filter=blob:none https://github.com/user/repo.git

# Sparse checkout
git config core.sparseCheckout true
echo "src/" > .git/info/sparse-checkout
git read-tree -m -u HEAD
```

### Git LFS for Large Files

```bash
# Install Git LFS
git lfs install

# Track large files
git lfs track "*.psd"
git lfs track "*.zip"

# Check tracked files
git lfs ls-files

# Push LFS files
git add .gitattributes
git commit -m "Add LFS tracking"
git push origin main
```

---

## Final Tips and Best Practices

### Commit Best Practices

1. **Make atomic commits** - Each commit should represent one logical change
2. **Write clear commit messages** - Use imperative mood ("Add feature" not "Added feature")
3. **Commit frequently** - Small, frequent commits are easier to manage
4. **Review before committing** - Use `git diff --staged` to review changes

### Branch Management

1. **Use descriptive branch names** - `feature/user-authentication` not `fix`
2. **Keep branches short-lived** - Merge or delete when done
3. **Protect main branch** - Use branch protection rules on GitHub
4. **Regular cleanup** - Delete merged branches

### Collaboration Guidelines

1. **Pull before push** - Always get latest changes first
2. **Use Pull Requests** - Don't push directly to main branch
3. **Review code carefully** - Take time to review PRs thoroughly
4. **Communicate changes** - Use descriptive PR descriptions

### Repository Hygiene

1. **Use .gitignore** - Don't commit generated files
2. **Regular maintenance** - Run `git gc` periodically
3. **Monitor repository size** - Use Git LFS for large files
4. **Backup regularly** - Push to multiple remotes if critical

### Learning Resources

- **Official Git Documentation**: https://git-scm.com/doc
- **Pro Git Book**: https://git-scm.com/book
- **GitHub Learning Lab**: https://lab.github.com/
- **Interactive Git Tutorial**: https://learngitbranching.js.org/

---

## Quick Reference Cheat Sheet

### Essential Commands

```bash
# Repository setup
git init
git clone <url>

# Basic workflow
git status
git add <file>
git commit -m "message"
git push
git pull

# Branching
git branch
git checkout <branch>
git merge <branch>

# Remote repositories
git remote -v
git fetch
git push origin <branch>

# Viewing history
git log
git log --oneline
git show <commit>

# Undoing changes
git checkout -- <file>
git reset HEAD <file>
git revert <commit>
```

### Advanced Commands

```bash
# Interactive rebase
git rebase -i HEAD~n

# Stashing
git stash
git stash pop

# Tagging
git tag -a v1.0 -m "message"

# Searching
git grep "search term"
git log --grep="search"

# Debugging
git bisect start
git blame <file>
git reflog
```
