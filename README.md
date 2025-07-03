# Complete Git Course Guide

## Table of Contents

1. [Introducing Git](#introducing-git)
2. [Installation & Setup](#installation--setup)
3. [The Very Basics Of Git: Adding & Committing](#the-very-basics-of-git-adding--committing)
4. [Commits In Detail (And Related Topics)](#commits-in-detail-and-related-topics)
5. [Working With Branches](#working-with-branches)
6. [Merging Branches, Oh Boy!](#merging-branches-oh-boy)
7. [Comparing Changes With Git Diff](#comparing-changes-with-git-diff)
8. [The Ins and Outs of Stashing](#the-ins-and-outs-of-stashing)
9. [Undoing Changes & Time Traveling](#undoing-changes--time-traveling)
10. [GitHub: The Basics](#github-the-basics)
11. [Fetching & Pulling](#fetching--pulling)
12. [GitHub Grab Bag: Odds & Ends](#github-grab-bag-odds--ends)
13. [Git Collaboration Workflows](#git-collaboration-workflows)
14. [Rebasing: The Scariest Git Command?](#rebasing-the-scariest-git-command)
15. [Cleaning Up History With Interactive Rebase](#cleaning-up-history-with-interactive-rebase)
16. [Git Tags: Marking Important Moments In History](#git-tags-marking-important-moments-in-history)
17. [The Power of Reflogs - Retrieving "Lost" Work](#the-power-of-reflogs---retrieving-lost-work)

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
# To amend any commit message, requires rebase
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
git revert HEAD~3
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

#### Remove stored credentials (if using credential manager)

```bash
git config --global --unset credential.helper
# Or on Windows/Mac, clear from credential manager

# Push again - you'll be prompted for username/password
git push origin main
```

### Repository Operations

```bash
# Clone repository
git clone https://github.com/username/repo-name.git
git clone git@github.com:username/repo-name.git  # SSH

# Add remote origin
git remote add origin https://github.com/username/repo-name.git

# update repo url
git remote set-url origin https://github.com/username/repository.git

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

#### After fetching, you can review changes before deciding to merge:

```bash
git fetch origin
git diff HEAD origin/main    # See what changed
git merge origin/main        # Merge when ready
```

#### Use fetch to download branch created by others and checkout to that branch

```bash
# First, fetch all remote branches
git fetch origin

# Create and switch to local auth branch tracking the remote
git checkout -b auth origin/auth
```

#### Pull (Fetch + Merge) - Only for current branch

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
```

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
# fixup - like squash; combines the commit with the previous commit but automatically discards the fixup commit's message, keeping only the previous commit's message
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

# Complete Git Rebase Tutorial

## Step 1: Create Repository and Initial Setup

```bash
# Create and initialize repository
mkdir git-rebase-demo
cd git-rebase-demo
git init
```

**Output:**

```
Initialized empty Git repository in /path/to/git-rebase-demo/.git/
```

## Step 2: Create Initial Commits on Main Branch

```bash
# Create first commit
echo "# My Project" > README.md
git add README.md
git commit -m "Initial commit: Add README"

# Create second commit
echo "print('Hello World')" > main.py
git add main.py
git commit -m "Add main.py file"

# Create third commit
echo "# Installation" >> README.md
git add README.md
git commit -m "Update README with installation"
```

**Check commit history:**

```bash
git log --oneline
```

**Output:**

```
c3d4e5f Update README with installation
b2c3d4e Add main.py file
a1b2c3d Initial commit: Add README
```

## Step 3: Create Feature Branch

```bash
# Create and switch to feature branch
git checkout -b feature/user-input
```

**Output:**

```
Switched to a new branch 'feature/user-input'
```

## Step 4: Add Commits to Feature Branch

```bash
# First feature commit
echo "name = input('Enter your name: ')" >> main.py
git add main.py
git commit -m "Add user input for name"

# Second feature commit (with typo)
echo "print('Hello', nam)" >> main.py
git add main.py
git commit -m "Add greeting with typo"

# Third feature commit (fix typo)
sed -i 's/nam/name/' main.py
git add main.py
git commit -m "Fix typo in greeting"

# Fourth feature commit
echo "print('Welcome to our app!')" >> main.py
git add main.py
git commit -m "Add welcome message"
```

**Check feature branch history:**

```bash
git log --oneline
```

**Output:**

```
h7i8j9k Add welcome message
g6h7i8j Fix typo in greeting
f5g6h7i Add greeting with typo
e4f5g6h Add user input for name
c3d4e5f Update README with installation
b2c3d4e Add main.py file
a1b2c3d Initial commit: Add README
```

## Step 5: Meanwhile, Main Branch Gets Updates

```bash
# Switch back to main
git checkout main

# Add new commit to main
echo "Requirements: Python 3.6+" > requirements.txt
git add requirements.txt
git commit -m "Add requirements file"
```

**Check main branch:**

```bash
git log --oneline
```

**Output:**

```
d4e5f6g Add requirements file
c3d4e5f Update README with installation
b2c3d4e Add main.py file
a1b2c3d Initial commit: Add README
```

## Step 6: Interactive Rebase to Clean Up Feature Branch

```bash
# Switch back to feature branch
git checkout feature/user-input

# Start interactive rebase for last 4 commits
git rebase -i HEAD~4
```

**Editor opens with:**

```
pick e4f5g6h Add user input for name
pick f5g6h7i Add greeting with typo
pick g6h7i8j Fix typo in greeting
pick h7i8j9k Add welcome message

# Rebase c3d4e5f..h7i8j9k onto c3d4e5f (4 commands)
```

**Edit to:**

```
pick e4f5g6h Add user input for name
squash f5g6h7i Add greeting with typo
squash g6h7i8j Fix typo in greeting
pick h7i8j9k Add welcome message
```

**Save and close editor. New editor opens for commit message:**

```
# This is a combination of 3 commits.
# This is the 1st commit message:

Add user input for name

# This is the commit message #2:

Add greeting with typo

# This is the commit message #3:

Fix typo in greeting
```

**Edit to:**

```
Add user input and greeting functionality

- Prompt user for their name
- Display personalized greeting
```

**Output after rebase:**

```
Successfully rebased and updated refs/heads/feature/user-input.
```

**Check cleaned history:**

```bash
git log --oneline
```

**Output:**

```
i8j9k0l Add welcome message
j9k0l1m Add user input and greeting functionality
c3d4e5f Update README with installation
b2c3d4e Add main.py file
a1b2c3d Initial commit: Add README
```

## Step 7: Rebase Feature Branch onto Updated Main

```bash
# Rebase feature branch onto main
git rebase main
```

**Output:**

```
Successfully rebased and updated refs/heads/feature/user-input.
```

**Check final history:**

```bash
git log --oneline
```

**Output:**

```
k0l1m2n Add welcome message
l1m2n3o Add user input and greeting functionality
d4e5f6g Add requirements file
c3d4e5f Update README with installation
b2c3d4e Add main.py file
a1b2c3d Initial commit: Add README
```

## Step 8: Verify the Changes

```bash
# Check the final main.py content
cat main.py
```

**Output:**

```
print('Hello World')
name = input('Enter your name: ')
print('Hello', name)
print('Welcome to our app!')
```

**Check branch structure:**

```bash
git log --oneline --graph --all
```

**Output:**

```
* k0l1m2n (HEAD -> feature/user-input) Add welcome message
* l1m2n3o Add user input and greeting functionality
* d4e5f6g (main) Add requirements file
* c3d4e5f Update README with installation
* b2c3d4e Add main.py file
* a1b2c3d Initial commit: Add README
```

## Step 9: Merge Back to Main

```bash
# Switch to main and merge
git checkout main
git merge feature/user-input
```

**Output:**

```
Updating d4e5f6g..k0l1m2n
Fast-forward
 main.py | 3 +++
 1 file changed, 3 insertions(+)
```

**Final check:**

```bash
git log --oneline
```

**Output:**

```
k0l1m2n Add welcome message
l1m2n3o Add user input and greeting functionality
d4e5f6g Add requirements file
c3d4e5f Update README with installation
b2c3d4e Add main.py file
a1b2c3d Initial commit: Add README
```

## Summary

- **Interactive rebase** (`git rebase -i`) cleaned up messy commits by squashing related changes
- **Branch rebase** (`git rebase main`) moved feature commits on top of updated main branch
- Result: Clean, linear history with logical commit groupings
- **Fast-forward merge** was possible due to rebasing

## Common Rebase Commands Reference

- `git rebase -i HEAD~n` - Interactive rebase last n commits
- `git rebase <branch>` - Rebase current branch onto target branch
- `git rebase --continue` - Continue after resolving conflicts
- `git rebase --abort` - Cancel rebase operation
- `git push --force-with-lease` - Safely push rebased commits

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
