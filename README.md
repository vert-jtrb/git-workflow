# GitFlow Best Practices - Step by Step Guide
blob:https://skillbuilder.aws/e90c12ac-7b05-43f1-88d7-494e26efff6a
## Current Situation
You're in: `~/DADTeam/training` on branch `feature1`
You have: `CommitTest.txt`

---

## GitFlow Branch Model Overview

```
main (production-ready code)
  │
  ├── develop (integration branch)
  │     │
  │     ├── feature/feature1 (new features)
  │     ├── feature/feature2
  │     │
  │     └── release/v1.0 (prepare releases)
  │           │
  │           └── hotfix/critical-bug (urgent fixes)
```

### Branch Types:
- **main** - Production code, always stable
- **develop** - Integration branch for features
- **feature/** - New features (branch from develop)
- **release/** - Release preparation (branch from develop)
- **hotfix/** - Emergency fixes (branch from main)

---

## Step 1: Check Your Current Status

```bash
# See where you are
git status

# See all branches
git branch -a

# See commit history
git log --oneline --graph --all
```

---

## Step 2: Setup GitFlow Structure

### Create main and develop branches (if not exist)

```bash
# Make sure you're on feature1
git branch

# Check if main exists
git show-ref --verify --quiet refs/heads/main || echo "main doesn't exist"

# If you're starting fresh, let's set up properly
# First, let's see what you have
git log --oneline
```

### Proper GitFlow Setup from Scratch

```bash
# 1. Create and switch to main branch (if not exists)
git checkout -b main

# 2. Add your initial file
git add CommitTest.txt
git commit -m "Initial commit: Project setup"

# 3. Create develop branch from main
git checkout -b develop

# 4. Now create your feature branch from develop
git checkout -b feature/add-user-authentication
```

---

## Step 3: Work on Feature Branch (Where You Are Now)

### Current: You're on `feature1`

Let's rename it to follow naming convention and work properly:

```bash
# Check current branch
git branch

# Rename feature1 to follow convention
git branch -m feature1 feature/commit-test

# Now you're on feature/commit-test
```

### Add Some Work to Your Feature

```bash
# Edit your file
echo "Testing commit workflow" >> CommitTest.txt

# Check status
git status

# Stage changes
git add CommitTest.txt

# Commit with descriptive message
git commit -m "feat: Add commit test content"

# Create another file for the feature
echo "Feature documentation" > FeatureDoc.md
git add FeatureDoc.md
git commit -m "docs: Add feature documentation"
```

---

## Step 4: Complete Feature and Merge to Develop

```bash
# 1. Switch to develop branch
git checkout develop

# 2. Merge feature branch (no fast-forward for history)
git merge --no-ff feature/commit-test -m "Merge feature/commit-test into develop"

# 3. View the merge
git log --oneline --graph --all

# 4. Delete feature branch (clean up)
git branch -d feature/commit-test
```

---

## Step 5: Create Another Feature (Parallel Development)

```bash
# From develop branch
git checkout develop

# Create new feature
git checkout -b feature/add-login-page

# Add some work
echo "<!DOCTYPE html>
<html>
<head><title>Login</title></head>
<body>
    <h1>Login Page</h1>
    <form>
        <input type='text' placeholder='Username'>
        <input type='password' placeholder='Password'>
        <button>Login</button>
    </form>
</body>
</html>" > login.html

git add login.html
git commit -m "feat: Create login page HTML structure"

# Add styling
echo "body { font-family: Arial; }
form { max-width: 300px; margin: 50px auto; }
input { display: block; width: 100%; margin: 10px 0; padding: 10px; }
button { width: 100%; padding: 10px; background: blue; color: white; }" > style.css

git add style.css
git commit -m "style: Add CSS styling for login page"
```

---

## Step 6: Merge Second Feature

```bash
# Switch to develop
git checkout develop

# Merge feature
git merge --no-ff feature/add-login-page -m "Merge feature/add-login-page into develop"

# Clean up
git branch -d feature/add-login-page
```

---

## Step 7: Create Release Branch

```bash
# From develop, create release branch
git checkout develop
git checkout -b release/v1.0.0

# Prepare release (version bump, changelog, etc.)
echo "# Changelog

## Version 1.0.0 (2024-11-19)

### Features
- Added commit test functionality
- Created login page with styling

### Documentation
- Added feature documentation" > CHANGELOG.md

git add CHANGELOG.md
git commit -m "docs: Add changelog for v1.0.0"

# Update version file
echo "1.0.0" > VERSION.txt
git add VERSION.txt
git commit -m "chore: Bump version to 1.0.0"
```

---

## Step 8: Merge Release to Main and Develop

```bash
# 1. Merge to main
git checkout main
git merge --no-ff release/v1.0.0 -m "Release v1.0.0"

# 2. Tag the release
git tag -a v1.0.0 -m "Release version 1.0.0"

# 3. Merge back to develop (for version updates)
git checkout develop
git merge --no-ff release/v1.0.0 -m "Merge release/v1.0.0 back to develop"

# 4. Delete release branch
git branch -d release/v1.0.0
```

---

## Step 9: Hotfix Branch (Emergency Fix)

```bash
# Branch from main for hotfix
git checkout main
git checkout -b hotfix/fix-login-bug

# Fix the bug
echo "// Fixed authentication bug" >> login.html
git add login.html
git commit -m "fix: Resolve login authentication bug"

# Update version
echo "1.0.1" > VERSION.txt
git add VERSION.txt
git commit -m "chore: Bump version to 1.0.1"
```

---

## Step 10: Merge Hotfix

```bash
# 1. Merge to main
git checkout main
git merge --no-ff hotfix/fix-login-bug -m "Hotfix v1.0.1"

# 2. Tag the hotfix
git tag -a v1.0.1 -m "Hotfix version 1.0.1"

# 3. Merge to develop
git checkout develop
git merge --no-ff hotfix/fix-login-bug -m "Merge hotfix/fix-login-bug to develop"

# 4. Delete hotfix branch
git branch -d hotfix/fix-login-bug
```

---

## Step 11: Push Everything to Remote

```bash
# Add remote (if not already added)
git remote add origin https://github.com/yourusername/training.git

# Push all branches
git push -u origin main
git push -u origin develop

# Push tags
git push --tags

# Push feature branch (if still working on it)
git push -u origin feature/your-feature-name
```

---

## GitFlow Commit Message Conventions

### Format: `<type>(<scope>): <subject>`

**Types:**
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `style`: Code style (formatting, no logic change)
- `refactor`: Code refactoring
- `test`: Adding tests
- `chore`: Maintenance tasks

**Examples:**
```bash
git commit -m "feat(auth): Add user login functionality"
git commit -m "fix(api): Resolve null pointer in user endpoint"
git commit -m "docs(readme): Update installation instructions"
git commit -m "style(css): Format login page styles"
git commit -m "refactor(utils): Simplify date formatting function"
git commit -m "test(auth): Add unit tests for login"
git commit -m "chore(deps): Update dependencies"
```

---

## Best Practices Summary

### ✅ DO:
1. **Always branch from develop** for features
2. **Use descriptive branch names**: `feature/add-user-profile`
3. **Commit often** with clear messages
4. **Use `--no-ff`** when merging to preserve history
5. **Delete branches** after merging
6. **Tag releases** on main branch
7. **Keep main stable** - only merge tested code
8. **Pull before push** to avoid conflicts
9. **Review before merging** (use pull requests)
10. **Keep commits atomic** (one logical change per commit)

### ❌ DON'T:
1. **Don't commit to main directly**
2. **Don't use generic messages** like "fix" or "update"
3. **Don't mix unrelated changes** in one commit
4. **Don't leave branches unmerged** for too long
5. **Don't push broken code** to develop
6. **Don't commit sensitive data** (passwords, keys)
7. **Don't rewrite public history** (avoid force push)
8. **Don't skip testing** before merging to main

---

## Quick Reference Commands

```bash
# Start new feature
git checkout develop
git pull origin develop
git checkout -b feature/my-feature

# Complete feature
git checkout develop
git merge --no-ff feature/my-feature
git branch -d feature/my-feature
git push origin develop

# Start release
git checkout -b release/v1.0.0 develop
# ... prepare release ...
git checkout main
git merge --no-ff release/v1.0.0
git tag -a v1.0.0 -m "Version 1.0.0"
git checkout develop
git merge --no-ff release/v1.0.0
git branch -d release/v1.0.0

# Hotfix
git checkout -b hotfix/critical-fix main
# ... fix bug ...
git checkout main
git merge --no-ff hotfix/critical-fix
git tag -a v1.0.1 -m "Hotfix 1.0.1"
git checkout develop
git merge --no-ff hotfix/critical-fix
git branch -d hotfix/critical-fix

# View everything
git log --oneline --graph --all --decorate
```

---

## Visual Branch History

```bash
# See your beautiful GitFlow history
git log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --all
```

---

## YOUR CURRENT SITUATION - Step by Step Guide

**Current branches:** main, feature1, debug2  
**Current location:** feature1 branch  
**Files:** CommitTest.txt

### Step 1: Check Current Status
```bash
# See what changes you have
git status

# See commit history
git log --oneline --all --graph
```

### Step 2: Create Develop Branch (Core of GitFlow)
```bash
# Switch to main first
git checkout main

# Create develop branch from main
git checkout -b develop

# Push develop to remote (if you have remote)
git push -u origin develop
```

### Step 3: Rename Your Feature Branches (Follow Convention)
```bash
# Rename feature1 to follow naming convention
git branch -m feature1 feature/commit-test

# Rename debug2 to follow naming convention
git branch -m debug2 feature/debug-improvements

# Check your branches now
git branch
```

### Step 4: Work on Your Feature Branch
```bash
# Switch to your feature
git checkout feature/commit-test

# Make sure your file is committed
git status

# If changes are not committed:
git add CommitTest.txt
git commit -m "feat: Add commit test functionality"

# Add more work to your feature
echo "Additional test content" >> CommitTest.txt
git add CommitTest.txt
git commit -m "feat: Enhance commit test with additional content"
```

### Step 5: Merge Feature to Develop
```bash
# Switch to develop
git checkout develop

# Merge your feature (use --no-ff to preserve history)
git merge --no-ff feature/commit-test -m "Merge feature/commit-test into develop"

# Check the merge
git log --oneline --graph --all

# Delete the feature branch (it's merged now)
git branch -d feature/commit-test
```

### Step 6: Work on Second Feature (debug-improvements)
```bash
# Make sure you're on develop
git checkout develop

# Switch to your debug feature
git checkout feature/debug-improvements

# Add some work
echo "# Debug Improvements

- Enhanced error logging
- Added debug mode flag
- Improved stack traces" > DEBUG.md

git add DEBUG.md
git commit -m "feat: Add debug improvements documentation"

# Create actual debug code
echo "def debug_log(message):
    print(f'[DEBUG] {message}')

def enable_debug_mode():
    import logging
    logging.basicConfig(level=logging.DEBUG)" > debug_utils.py

git add debug_utils.py
git commit -m "feat: Implement debug utility functions"
```

### Step 7: Merge Second Feature
```bash
# Switch to develop
git checkout develop

# Merge debug feature
git merge --no-ff feature/debug-improvements -m "Merge feature/debug-improvements into develop"

# Delete merged branch
git branch -d feature/debug-improvements

# Check your beautiful history
git log --oneline --graph --all
```

### Step 8: Create a Release
```bash
# From develop, create release branch
git checkout develop
git checkout -b release/v1.0.0

# Add version file
echo "1.0.0" > VERSION.txt
git add VERSION.txt
git commit -m "chore: Bump version to 1.0.0"

# Add changelog
echo "# Changelog

## Version 1.0.0 ($(date +%Y-%m-%d))

### Features
- Added commit test functionality
- Implemented debug improvements
- Added debug utility functions

### Documentation
- Added debug improvements documentation" > CHANGELOG.md

git add CHANGELOG.md
git commit -m "docs: Add changelog for v1.0.0"
```

### Step 9: Finalize Release
```bash
# Merge to main
git checkout main
git merge --no-ff release/v1.0.0 -m "Release v1.0.0"

# Tag the release
git tag -a v1.0.0 -m "Release version 1.0.0"

# Merge back to develop
git checkout develop
git merge --no-ff release/v1.0.0 -m "Merge release/v1.0.0 back to develop"

# Delete release branch
git branch -d release/v1.0.0

# Check your branches
git branch
```

### Step 10: Push Everything to Remote (If You Have Remote)
```bash
# Check if remote exists
git remote -v

# If remote exists, push everything
git push origin main
git push origin develop
git push --tags

# If no remote, add one first:
# git remote add origin https://github.com/yourusername/training.git
# Then push
```

### Your GitFlow is Now Complete! 🎉

```bash
# View your complete history
git log --oneline --graph --all --decorate

# See all branches
git branch -a

# See all tags
git tag
```
