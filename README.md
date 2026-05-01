# Complete Git Self-Help Guide

## Table of Contents

1. [Introducing Git](#introducing-git)
2. [Installation & Setup](#installation--setup)
3. [The Very Basics: Adding & Committing](#the-very-basics-adding--committing)
4. [Commits In Detail](#commits-in-detail)
5. [Working With Branches](#working-with-branches)
6. [Merging Branches](#merging-branches)
7. [Comparing Changes With Git Diff](#comparing-changes-with-git-diff)
8. [Stashing](#stashing)
9. [Undoing Changes & Time Traveling](#undoing-changes--time-traveling)
10. [GitHub: The Basics](#github-the-basics)
11. [Fetching & Pulling](#fetching--pulling)
12. [GitHub Basics Part 2](#github-basics-part-2)
13. [Git Collaboration Workflows](#git-collaboration-workflows)
14. [Rebasing](#rebasing)
15. [Interactive Rebase](#interactive-rebase)
16. [Git Tags](#git-tags)
17. [Reflogs - Retrieving Lost Work](#reflogs---retrieving-lost-work)

---

## Introducing Git

### What is Git?

Git is a **distributed version control system** created by Linus Torvalds in 2005. It tracks changes in files and coordinates work among multiple people.

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

**Windows**: Download from [git-scm.com](https://git-scm.com), run installer with default settings, open Git Bash or Command Prompt.

**macOS**:

```bash
brew install git
```

**Linux (Ubuntu/Debian)**:

```bash
sudo apt update
sudo apt install git
```

### Initial Configuration

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
git config --global init.defaultBranch main
git config --list
```

### Verify Installation

```bash
git --version
```

### Set VS Code as Default Editor

```bash
git config --global core.editor "code --wait"
```

---

## The Very Basics: Adding & Committing

### Initializing a Repository

```bash
mkdir my-project
cd my-project
git init
```

### The Three Areas

1. **Working Directory**: Your actual files on disk
2. **Staging Area**: Files prepared for the next commit
3. **Repository**: Committed snapshots stored in `.git/`

### Basic Workflow

```bash
git status

git add filename.txt          # Stage specific file
git add .                     # Stage all files
git add *.js                  # Stage all JavaScript files

git commit -m "Initial commit"

git log
git log --oneline             # Condensed view
```

### Example Workflow

1. Create a file called `hello.txt` with the content `Hello Git!`

2. Check status — it shows as untracked:

```bash
git status
```

3. Stage and commit:

```bash
git add hello.txt
git commit -m "Add hello.txt file"
git log --oneline
```

---

## Commits In Detail

### Anatomy of a Commit

Each commit contains:

- **SHA-1 Hash**: Unique identifier (e.g., `a1b2c3d4...`)
- **Author**: Name and email
- **Date**: When the commit was made
- **Message**: Description of changes
- **Parent**: Reference to previous commit(s)

### Writing Good Commit Messages

```bash
# Good
git commit -m "Add user authentication feature"
git commit -m "Fix memory leak in image processing"

# Bad
git commit -m "stuff"
git commit -m "fix"
```

### Commit Message Conventions

```bash
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
git show
git show <commit-hash>
git show --name-only <commit-hash>
git show --stat <commit-hash>
```

---

## Working With Branches

### What are Branches?

Branches let you work on different features or experiments without affecting the main codebase. Think of them as parallel universes of your project.

### Branch Commands

```bash
git branch                            # List all branches

git branch feature-login              # Create new branch

git switch feature-login              # Switch to branch
git checkout feature-login            # Older syntax

git switch -c feature-signup          # Create and switch
git checkout -b feature-signup        # Older syntax

git branch -d feature-login           # Safe delete
git branch -D feature-login           # Force delete
```

### Branch Workflow Example

1. Start on the main branch:

```bash
git checkout main
```

2. Create a feature branch:

```bash
git checkout -b add-shopping-cart
```

3. Create a file called `cart.js` with your shopping cart code, then stage and commit:

```bash
git add cart.js
git commit -m "Add shopping cart feature"
```

4. Switch back to main — the `cart.js` file won't be visible here:

```bash
git checkout main
```

### Renaming Branches

```bash
git branch -m new-branch-name         # Rename current branch
git branch -m old-name new-name       # Rename specific branch
```

---

## Merging Branches

### Types of Merges

**Fast-Forward Merge** — when the target branch hasn't changed since branching:

```bash
git checkout main
git merge feature-branch
```

**Three-Way Merge** — when both branches have new commits (creates a merge commit):

```bash
git merge feature-branch
```

### Merge Example

1. Create and work on a feature branch:

```bash
git checkout -b feature-navbar
```

2. Create a file called `navbar.js` with navigation bar code, then commit:

```bash
git add navbar.js
git commit -m "Add navigation bar"
```

3. Switch to main, create a file called `main.js` with main content, and commit:

```bash
git checkout main
git add main.js
git commit -m "Add main content"
```

4. Merge the feature branch (creates a merge commit since both branches diverged):

```bash
git merge feature-navbar
```

### Merge Conflicts

When Git can't automatically merge, it marks conflicts in the file:

```
<<<<<<< HEAD
Content from current branch
=======
Content from feature branch
>>>>>>> feature-branch
```

Resolve by editing the file to keep the desired content, removing the markers, then:

```bash
git add file.txt
git commit
```

### Merge Strategies

```bash
git merge --no-ff feature-branch      # Always create merge commit
git merge --squash feature-branch     # Combine all commits into one
git commit -m "Add feature XYZ"
```

### Merge a Single File from Another Branch

```bash
git checkout main
git fetch origin
git checkout branch_name -- path/to/your/file
git add path/to/your/file
git commit -m "Merged file from branch_name into main"
```

---

## Comparing Changes With Git Diff

### Basic Diff Commands

```bash
git diff                              # Unstaged changes
git diff --staged                     # Staged changes
git diff --cached                     # Same as --staged
git diff filename.txt                 # Specific file
git diff main feature-branch          # Between branches
git diff commit1 commit2              # Between commits
```

### Diff Output Explained

```diff
diff --git a/file.txt b/file.txt
--- a/file.txt
+++ b/file.txt
@@ -1,3 +1,4 @@
 line 1
-line 2
+line 2 modified
+new line 3
 line 4
```

- Lines starting with `-` were removed
- Lines starting with `+` were added
- Lines starting with a space are unchanged context

### Advanced Diff Options

```bash
git diff --word-diff                  # Word-level diff
git diff --name-only                  # Only changed file names
git diff --stat                       # Statistics
git diff --ignore-space-change        # Ignore whitespace
```

---

## Stashing

### What is Stashing?

Stashing temporarily saves your uncommitted changes so you can work on something else and re-apply them later.

### Basic Stashing

```bash
git stash                             # Stash current changes
git stash list                        # List all stashes
git stash pop                         # Apply most recent stash and remove it
git stash apply                       # Apply stash without removing it
git stash drop                        # Drop a stash
```

### Advanced Stashing

```bash
git stash push -m "WIP: user login"   # Stash with message
git stash push -m "Partial" f1.txt    # Stash specific files
git stash -u                          # Include untracked files
git stash apply stash@{2}             # Apply specific stash
git stash show -p stash@{0}           # Show stash contents
```

### Stashing Workflow Example

1. You're working on a feature — you've modified `feature.js` and staged it.

2. An urgent bug fix is needed. Stash your work:

```bash
git stash push -m "Incomplete login feature"
```

3. Switch to main, fix the bug, commit:

```bash
git checkout main
```

Create `bugfix.js` with the fix, then:

```bash
git add bugfix.js
git commit -m "Fix critical bug"
```

4. Return to feature branch and restore your stash:

```bash
git checkout feature-branch
git stash pop
```

---

## Undoing Changes & Time Traveling

### Undoing Working Directory Changes

```bash
git restore filename.txt              # Discard changes (new syntax)
git checkout -- filename.txt          # Discard changes (old syntax)
git restore .                         # Discard all changes
```

### Undoing Staged Changes

```bash
git restore --staged filename.txt     # Unstage (new syntax)
git reset HEAD filename.txt           # Unstage (old syntax)
git reset HEAD                        # Unstage all
```

### Undoing Commits

**Reset** (rewrites history — use only on local/unpushed commits):

```bash
git reset --soft HEAD~1               # Keep changes staged
git reset --mixed HEAD~1              # Keep changes unstaged (default)
git reset HEAD~1                      # Same as --mixed
git reset --hard HEAD~1               # Discard all changes
```

**Revert** (safe for shared repos — creates a new commit that undoes a previous one):

```bash
git revert HEAD                       # Undo last commit
git revert <commit-hash>              # Undo specific commit
```

### When to Use Which

- **Reset**: You haven't pushed yet and want to redo commits
- **Revert**: You've already pushed and need to undo safely

---

## GitHub: The Basics

### What is GitHub?

GitHub is a cloud-based Git hosting service that adds collaboration features, issue tracking, and project management.

### SSH Key Setup (Recommended)

```bash
ssh-keygen -t ed25519 -C "your.email@example.com"
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
cat ~/.ssh/id_ed25519.pub
```

Copy the output and add it to **GitHub → Settings → SSH Keys**.

### HTTPS with Personal Access Token

1. GitHub Settings → Developer settings → Personal access tokens
2. Generate token with appropriate permissions
3. Use token as password when prompted

### Remove Stored Credentials

```bash
git config --global --unset credential.helper
git push origin main                  # You'll be prompted again
```

### Repository Operations

```bash
git clone https://github.com/user/repo.git       # HTTPS
git clone git@github.com:user/repo.git            # SSH

git remote add origin https://github.com/user/repo.git
git remote set-url origin https://github.com/user/repo.git
git remote -v                                      # Check remotes

git push -u origin main               # Set upstream + push
git push                              # After upstream is set
```

### GitHub Workflow

1. Clone or create repo
2. Create feature branch: `git checkout -b feature-new-header`
3. Create your files, stage, and commit
4. Push branch: `git push origin feature-new-header`
5. Create Pull Request on GitHub
6. Merge after review
7. Delete branch: `git branch -d feature-new-header`

---

## Fetching & Pulling

### Understanding Remote Tracking

```bash
git branch -r                         # Remote branches
git branch -a                         # All branches
git remote show origin                # Remote info
```

### Fetch (Download Without Merging)

```bash
git fetch                             # All remotes
git fetch origin                      # Specific remote
git fetch origin main                 # Specific branch
```

After fetching, review before merging:

```bash
git diff HEAD origin/main             # See what changed
git merge origin/main                 # Merge when ready
```

### Fetch and Checkout a Remote Branch

```bash
git fetch origin
git checkout -b auth origin/auth      # Track remote branch locally
```

### Pull (Fetch + Merge)

```bash
git pull                              # Current branch
git pull origin main                  # Specific branch
git pull --rebase                     # Rebase instead of merge
```

### Daily Collaboration Workflow

```bash
# Morning — get latest
git checkout main
git pull origin main

# Create feature branch
git checkout -b feature-user-profile

# Work, stage, commit...
git add .
git commit -m "Add user profile page"

# Before pushing — sync with main
git checkout main
git pull origin main
git checkout feature-user-profile
git rebase main

# Push
git push origin feature-user-profile
```

---

## GitHub Basics Part 2

### Forking Workflow

1. Fork repository on GitHub
2. Clone your fork:

```bash
git clone https://github.com/yourusername/forked-repo.git
```

3. Add upstream remote:

```bash
git remote add upstream https://github.com/original-owner/repo.git
```

4. Keep fork updated:

```bash
git fetch upstream
git checkout main
git merge upstream/main
git push origin main
```

### Pull Requests

- Write descriptive titles and descriptions
- Reference issues with `#issue-number`
- Request specific reviewers
- Use draft PRs for work in progress

### GitHub Pages

1. Create a `gh-pages` branch:

```bash
git checkout -b gh-pages
```

2. Create a file called `index.html` with your HTML content.

3. Stage, commit, and push:

```bash
git add index.html
git commit -m "Add GitHub Pages site"
git push origin gh-pages
```

Site available at: `https://username.github.io/repository-name`

### README Best Practices

A good README includes: project title, brief description, installation steps, usage examples, and contribution guidelines.

---

## Git Collaboration Workflows

### Centralized Workflow

Simple workflow for small teams — everyone works on the main branch:

```bash
git clone repo
git add .
git commit -m "Add feature"
git pull
git push
```

### Feature Branch Workflow

Most common workflow:

```bash
git checkout -b feature-user-auth
git add .
git commit -m "Implement user authentication"
git push origin feature-user-auth
# Create Pull Request → Review → Merge
git branch -d feature-user-auth
```

### Gitflow Workflow

Structured workflow for larger projects. Two permanent branches: `main` (production) and `develop`.

**Feature branches** branch from `develop`:

```bash
git checkout develop
git checkout -b feature/user-login
# Work on feature...
git checkout develop
git merge feature/user-login
```

**Release branches** branch from `develop`, merge into both `main` and `develop`:

```bash
git checkout -b release/1.2.0 develop
# Bug fixes only...
git checkout main
git merge release/1.2.0
git checkout develop
git merge release/1.2.0
```

**Hotfix branches** branch from `main`, merge into both `main` and `develop`:

```bash
git checkout -b hotfix/critical-bug main
# Fix bug...
git checkout main
git merge hotfix/critical-bug
git checkout develop
git merge hotfix/critical-bug
```

### Forking Workflow

For open source contributions:

```bash
git clone https://github.com/yourfork/project.git
git remote add upstream https://github.com/original/project.git
git checkout -b new-feature
# Work...
git push origin new-feature
# Create Pull Request from fork to original repo
```

---

## Rebasing

### What is Rebasing?

Rebasing re-applies commits from one branch onto another, creating a **linear history** instead of merge commits.

### Merge vs Rebase

**Merge** creates a merge commit:

```bash
git checkout main
git merge feature-branch
```

**Rebase** creates linear history:

```bash
git checkout feature-branch
git rebase main
git checkout main
git merge feature-branch              # Fast-forward merge
```

### Basic Rebase

```bash
git rebase main                       # Rebase current branch onto main
git rebase main feature-branch        # Rebase specific branch
```

### Rebase Visualized

```
Before rebase:
  main:    A---B---C
  feature:     D---E---F

After `git checkout feature && git rebase main`:
  main:    A---B---C
  feature:         D'---E'---F'
```

### Golden Rule

**Never rebase commits that have been pushed to a shared repository.**

### Handling Rebase Conflicts

```bash
git rebase main
# Conflict occurs — fix the file, then:
git add conflicted-file.txt
git rebase --continue

git rebase --skip                     # Skip problematic commit
git rebase --abort                    # Cancel entire rebase
```

---

## Interactive Rebase

### Starting Interactive Rebase

```bash
git rebase -i HEAD~4
```

Editor opens showing your last 4 commits with action keywords:

```
pick 1234567 Add login feature
pick 2345678 Fix typo in login
pick 3456789 Update login styling
pick 4567890 Debug logging
```

### Available Actions

- `pick` — use commit as is
- `reword` — change commit message
- `edit` — stop and let you modify the commit
- `squash` — combine with previous commit (edit combined message)
- `fixup` — combine with previous commit (discard this message)
- `drop` — remove commit entirely

### Squashing Commits

Combine multiple related commits into one:

```
pick 1234567 Add user authentication
squash 2345678 Fix authentication bug
squash 3456789 Add authentication tests
```

Result: One clean commit with all three changes.

### Reordering Commits

Simply rearrange the lines:

```
pick A Add feature X
pick C Fix feature X      ← moved up
pick B Add feature Y      ← moved down
```

### Editing a Commit

```
edit 1234567 Add user profile
pick 2345678 Update navigation
```

Git stops at the `edit` commit. Make changes, then:

```bash
git add modified-file.txt
git commit --amend
git rebase --continue
```

### Splitting a Commit

```
edit 1234567 Large commit to split
```

When Git stops:

```bash
git reset HEAD~1                      # Unstage changes
git add file1.txt
git commit -m "Add file1 functionality"
git add file2.txt
git commit -m "Add file2 functionality"
git rebase --continue
```

---

### Complete Rebase Walkthrough

**Step 1**: Create a new repository:

```bash
mkdir git-rebase-demo
cd git-rebase-demo
git init
```

**Step 2**: Create initial commits on main.

Create a file called `README.md` with the content `# My Project`, then:

```bash
git add README.md
git commit -m "Initial commit: Add README"
```

Create a file called `main.py` with the content `print('Hello World')`, then:

```bash
git add main.py
git commit -m "Add main.py file"
```

Append `# Installation` to `README.md`, then:

```bash
git add README.md
git commit -m "Update README with installation"
```

**Step 3**: Create a feature branch:

```bash
git checkout -b feature/user-input
```

**Step 4**: Add commits to the feature branch.

Append `name = input('Enter your name: ')` to `main.py`:

```bash
git add main.py
git commit -m "Add user input for name"
```

Append `print('Hello', nam)` to `main.py` (intentional typo):

```bash
git add main.py
git commit -m "Add greeting with typo"
```

Fix the typo (`nam` → `name`):

```bash
git add main.py
git commit -m "Fix typo in greeting"
```

Append `print('Welcome to our app!')` to `main.py`:

```bash
git add main.py
git commit -m "Add welcome message"
```

History now:

```
h7i8j9k Add welcome message
g6h7i8j Fix typo in greeting
f5g6h7i Add greeting with typo
e4f5g6h Add user input for name
c3d4e5f Update README with installation
b2c3d4e Add main.py file
a1b2c3d Initial commit: Add README
```

**Step 5**: Meanwhile, main gets an update. Switch to main:

```bash
git checkout main
```

Create a file called `requirements.txt` with the content `Requirements: Python 3.6+`, then:

```bash
git add requirements.txt
git commit -m "Add requirements file"
```

**Step 6**: Interactive rebase to clean up the feature branch:

```bash
git checkout feature/user-input
git rebase -i HEAD~4
```

Editor shows:

```
pick e4f5g6h Add user input for name
pick f5g6h7i Add greeting with typo
pick g6h7i8j Fix typo in greeting
pick h7i8j9k Add welcome message
```

Change to:

```
pick e4f5g6h Add user input for name
squash f5g6h7i Add greeting with typo
squash g6h7i8j Fix typo in greeting
pick h7i8j9k Add welcome message
```

Save. In the next editor, replace the combined message with:

```
Add user input and greeting functionality

- Prompt user for their name
- Display personalized greeting
```

Cleaned history:

```
i8j9k0l Add welcome message
j9k0l1m Add user input and greeting functionality
c3d4e5f Update README with installation
b2c3d4e Add main.py file
a1b2c3d Initial commit: Add README
```

**Step 7**: Rebase feature onto updated main:

```bash
git rebase main
```

**Step 8**: Merge back to main (fast-forward):

```bash
git checkout main
git merge feature/user-input
```

Final linear history:

```
k0l1m2n Add welcome message
l1m2n3o Add user input and greeting functionality
d4e5f6g Add requirements file
c3d4e5f Update README with installation
b2c3d4e Add main.py file
a1b2c3d Initial commit: Add README
```

### Common Rebase Commands Reference

| Command | Purpose |
|---|---|
| `git rebase -i HEAD~n` | Interactive rebase last n commits |
| `git rebase <branch>` | Rebase onto target branch |
| `git rebase --continue` | Continue after resolving conflicts |
| `git rebase --abort` | Cancel rebase |
| `git push --force-with-lease` | Safely push rebased commits |

---

## Git Tags

### What are Tags?

Tags mark specific points in history, typically used for releases.

### Lightweight Tags

```bash
git tag v1.0                          # Tag current commit
git tag v1.0 <commit-hash>            # Tag specific commit
```

### Annotated Tags (Recommended)

```bash
git tag -a v1.0 -m "Release version 1.0"
```

### Working with Tags

```bash
git tag                               # List all tags
git tag -l "v1.*"                     # Filter tags
git show v1.0                         # Show tag info
git checkout v1.0                     # Checkout tag
git tag -d v1.0                       # Delete tag
```

### Pushing Tags

```bash
git push origin v1.0                  # Push specific tag
git push origin --tags                # Push all tags
git push --follow-tags                # Push annotated tags only
```

### Semantic Versioning

Format: `Major.Minor.Patch`

```bash
git tag -a v1.0.0 -m "Initial release"
git tag -a v1.0.1 -m "Bug fix release"
git tag -a v1.1.0 -m "Minor feature release"
git tag -a v2.0.0 -m "Major release with breaking changes"
```

### Release Workflow

```bash
git checkout main
git pull origin main

# Update version numbers in your code files
git add .
git commit -m "Bump version to 1.2.0"

git tag -a v1.2.0 -m "Release version 1.2.0"

git push origin main
git push origin v1.2.0
# Create GitHub release from the tag
```

---

## Reflogs - Retrieving Lost Work

### What is Reflog?

Reflog (reference log) tracks every change to branch tips and HEAD. It's your safety net for recovering "lost" work.

### Viewing Reflog

```bash
git reflog                            # Current branch
git reflog feature-branch             # Specific branch
git reflog --all                      # All references
git reflog --date=iso                 # With dates
```

### Reflog Output Example

```
abc1234 HEAD@{0}: commit: Add user authentication
def5678 HEAD@{1}: checkout: moving from main to feature-auth
9012345 HEAD@{2}: commit: Update README
```

### Recovering Lost Commits

```bash
# Accidentally ran:
git reset --hard HEAD~3               # Lost 3 commits!

# Find them:
git reflog
# Output: abc1234 HEAD@{1}: commit: Important feature

# Recover:
git reset --hard abc1234
# Or create a new branch:
git branch recovery-branch abc1234
```

### Recovering Deleted Branches

```bash
# Accidentally deleted:
git branch -D important-feature

# Find it:
git reflog --all | grep important-feature

# Recreate:
git branch important-feature <commit-hash>
```

### Recovering Deleted Files

```bash
git log --oneline --follow -- deleted-file.txt
git checkout <commit-before-deletion>
git checkout -b recover-file
# File is present on this branch
```

### Reflog Expiration

Entries expire after 90 days by default.

```bash
git config gc.reflogExpire "never"
git config gc.reflogExpireUnreachable "never"
git reflog expire --expire=30.days refs/heads/main
```

---
