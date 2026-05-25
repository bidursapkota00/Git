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
9. [.gitignore](#gitignore)
10. [Undoing Changes & Time Traveling](#undoing-changes--time-traveling)
11. [GitHub: The Basics](#github-the-basics)
12. [Fetching & Pulling](#fetching--pulling)
13. [GitHub Basics Part 2](#github-basics-part-2)
14. [Git Collaboration Workflows](#git-collaboration-workflows)
15. [Rebasing](#rebasing)
16. [Interactive Rebase](#interactive-rebase)
17. [Cherry-Pick](#cherry-pick)
18. [Git Tags](#git-tags)
19. [Reflogs - Retrieving Lost Work](#reflogs---retrieving-lost-work)

---

## Introducing Git

Git is a distributed version control system created by Linus Torvalds in 2005. Every user has a complete copy of the project history. Your project lives in a repository, which holds both your files and all of Git's tracking information. As you work, you stage changes in a staging area and then save them as commits — snapshots of your project at a point in time. You can create branches to develop features in isolation and use remotes to sync your work with others.

Git lets you track the full history of your project, collaborate with multiple developers, experiment on separate branches without risk, revert to earlier versions when something breaks, and keep your code backed up across multiple locations.

---

## Installation & Setup

```bash
# macOS
brew install git

# Linux (Ubuntu/Debian)
sudo apt update && sudo apt install git

# Verify
git --version
```

**Windows**: Download from [git-scm.com](https://git-scm.com), run installer with default settings, open Git Bash or Command Prompt.

### Initial Configuration

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
git config --global init.defaultBranch main
git config --global core.editor "code --wait"
```

---

## The Very Basics: Adding & Committing

### The Three Areas

1. **Working Directory**
2. **Staging Area**
3. **Repository (.git/)**

### Basic Workflow

```bash
git init                              # Initialize repo
git status                            # Check status

git add filename.txt                  # Stage specific file
git add .                             # Stage all files

git commit -m "Initial commit"        # Commit

git log --oneline                     # View history
```

---

## Commits In Detail

Each commit has: SHA-1 hash, author, date, message, and parent reference.

### Writing Good Commit Messages

```bash
git commit -m "feat: add login functionality"
git commit -m "fix: resolve null pointer exception"
git commit -m "docs: update API documentation"
```

### Amending Commits

```bash
git commit --amend -m "New commit message"       # Fix message
git add forgotten-file.txt
git commit --amend --no-edit                      # Add forgotten files
```

### Viewing Commit Details

```bash
git show <commit-hash>
```

---

## Working With Branches

Branches let you work on features without affecting the main codebase.

```bash
git branch                            # List branches
git branch feature-login              # Create branch
git switch feature-login              # Switch to branch
git switch -c feature-signup          # Create and switch
git branch -d feature-login           # Delete (safe)
git branch -D feature-login           # Delete (force)
git branch -m old-name new-name       # Rename
```

---

## Merging Branches

### Fast-Forward vs Three-Way Merge

```bash
git checkout main
git merge feature-branch              # Fast-forward if possible
git merge --no-ff feature-branch      # Always create merge commit
git merge --squash feature-branch     # Squash into one commit
```

### Merge Conflicts

When Git can't auto-merge, it marks conflicts:

```
<<<<<<< HEAD
Content from current branch
=======
Content from feature branch
>>>>>>> feature-branch
```

Resolve by editing the file, removing markers, then:

```bash
git add file.txt
git commit
```

---

## Comparing Changes With Git Diff

```bash
git diff                              # Unstaged changes
git diff --staged                     # Staged changes
git diff main feature-branch          # Between branches
git diff commit1 commit2              # Between commits
git diff --name-only                  # Only changed filenames
git diff --stat                       # Statistics
```

**Reading output**: `-` = removed, `+` = added, space = unchanged context.

---

## Stashing

Temporarily save uncommitted changes to work on something else.

```bash
git stash                             # Stash changes
git stash push -m "WIP: user login"   # Stash with message
git stash -u                          # Include untracked files
git stash list                        # List stashes
git stash pop                         # Apply + remove most recent
git stash apply stash@{2}             # Apply specific stash
git stash drop                        # Drop a stash
```

---

## .gitignore

`.gitignore` tells Git which files to ignore. Common patterns:

```
.env
node_modules/
dist/
build/
.DS_Store
.vscode/
*.log
```

> **Note**: `.gitignore` only ignores untracked files. Already-tracked files need `git rm --cached filename` first.

---

## Undoing Changes & Time Traveling

### Discard Working Directory Changes

```bash
git restore filename.txt              # Discard changes
git restore .                         # Discard all
```

### Unstage Files

```bash
git restore --staged filename.txt
```

### Undo Commits

**Reset** (local/unpushed only — rewrites history):

```bash
git reset --soft HEAD~1               # Keep changes staged
git reset HEAD~1                      # Keep changes unstaged (default)
git reset --hard HEAD~1               # Discard everything
```

**Revert** (safe for shared repos — creates undo commit):

```bash
git revert <commit-hash>
```

### Detached HEAD

Happens when you checkout a specific commit. To save work from detached state:

```bash
git checkout -b <new-branch-name>
```

### Cleaning Untracked Files

```bash
git clean -dn                         # Preview
git clean -df                         # Remove
```

---

## GitHub: The Basics

GitHub is a cloud-based Git hosting service that adds collaboration features, issue tracking, and project management.

### Remove Stored Credentials

```bash
git config --global --unset credential.helper
git push origin main                  # Use brwoser based auth
```

### Repository Operations

```bash
git clone https://github.com/user/repo.git         # Clone
git remote add origin <url>
git remote -v                                      # Check remotes
git push -u origin main                            # Set upstream + push
git push                                           # After upstream is set
```

### Typical Workflow

1. Clone or create repo
2. Create feature branch: `git checkout -b feature-xyz`
3. Work, stage, commit
4. Push: `git push origin feature-xyz`
5. Create Pull Request → Review → Merge

---

## Fetching & Pulling

```bash
git fetch origin                      # Download without merging
git diff HEAD origin/main             # Review changes
git merge origin/main                 # Merge when ready

git pull                              # Fetch + merge (shortcut)
git pull --rebase                     # Fetch + rebase instead
```

### Remote Tracking

```bash
git branch -r                         # Remote branches
git branch -a                         # All branches
```

### Force Push After Rebase

```bash
git push --force-with-lease           # Safe force push
```

### Pushing to Different Remote Branch Name

```bash
git push -u origin local-branch:remote-branch
```

---

## GitHub Basics Part 2

### Forking Workflow

```bash
git clone https://github.com/yourusername/forked-repo.git
git remote add upstream https://github.com/original-owner/repo.git

# Keep fork updated:
git fetch upstream
git checkout main
git merge upstream/main
git push origin main
```

### GitHub Pages

Push to a `gh-pages` branch → site at `https://username.github.io/repo-name`.

---

## Git Collaboration Workflows

### Feature Branch Workflow (most common)

```bash
git checkout -b feature-user-auth
# Work, commit...
git push origin feature-user-auth
# Create Pull Request → Review → Merge
git branch -d feature-user-auth
```

### Gitflow Workflow

Two permanent branches: `main` (production) and `develop`. Feature/release/hotfix branches branch off and merge back:

```bash
# Feature
git checkout develop && git checkout -b feature/login
# ... work, then merge back into develop

# Release
git checkout -b release/1.2.0 develop
# ... merge into both main and develop

# Hotfix
git checkout -b hotfix/critical-bug main
# ... merge into both main and develop
```

---

## Rebasing

Rebasing re-applies commits onto another branch, creating linear history.

```bash
git checkout feature-branch
git rebase main                       # Rebase onto main
git checkout main
git merge feature-branch              # Fast-forward merge
```

**Golden Rule**: Never rebase commits already pushed to a shared repo.

### Handling Conflicts

```bash
# Fix conflicts, then:
git add conflicted-file.txt
git rebase --continue
git rebase --abort                    # Cancel rebase
```

---

## Interactive Rebase

```bash
git rebase -i HEAD~4
```

### Actions

| Action   | Effect                                        |
| -------- | --------------------------------------------- |
| `pick`   | Use commit as-is                              |
| `reword` | Change commit message                         |
| `squash` | Combine with previous (edit combined message) |
| `fixup`  | Combine with previous (discard this message)  |
| `drop`   | Remove commit                                 |
| `edit`   | Pause to amend the commit                     |

### Squashing Example

```
pick 1234567 Add user authentication
squash 2345678 Fix auth bug
squash 3456789 Add auth tests
```

Result: one clean commit with all three changes.

---

## Cherry-Pick

Apply changes from a specific commit onto your current branch.

```bash
git cherry-pick <commit-hash>
git cherry-pick <hash1> <hash2>       # Multiple commits
```

**Use cases**: backport bug fixes, apply a single commit without merging a whole branch, recover a commit from a deleted branch.

### Handling Conflicts

```bash
git add resolved-file.txt
git cherry-pick --continue
git cherry-pick --abort               # Cancel
```

---

## Git Tags

Tags mark specific points in history, typically for releases.

```bash
git tag -a v1.0.0 -m "Release 1.0.0" # Annotated tag (recommended)
git tag v1.0.0                        # Lightweight tag
git tag                               # List tags
git show v1.0.0                       # Show tag info
git tag -d v1.0.0                     # Delete tag

git push origin v1.0.0               # Push specific tag
git push origin --tags                # Push all tags
```

**Semantic Versioning**: `Major.Minor.Patch` — major = breaking changes, minor = new features, patch = bug fixes.

---

## Reflogs - Retrieving Lost Work

Reflog tracks every change to HEAD. Your safety net for recovering "lost" work (expires after 90 days).

```bash
git reflog                            # View reflog
```

### Recovering Lost Commits

```bash
git reset --hard HEAD~3               # Oops, lost 3 commits!
git reflog                            # Find the hash
git reset --hard <commit-hash>        # Recover
```

### Recovering Deleted Branches

```bash
git branch -D important-feature       # Oops!
git reflog --all | grep important-feature
git branch important-feature <commit-hash>
```
