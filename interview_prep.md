# Top 15 GitHub/Code Repository Interview Questions with Detailed Explanations
---

## 1. **What is Git and how is it different from GitHub?**

**Explanation:**

**Git:**
- A distributed version control system (software you install on your computer)
- Tracks changes in your code over time
- Works locally on your machine
- Created by Linus Torvalds in 2005
- Free and open-source

**GitHub:**
- A cloud-based hosting service for Git repositories
- Provides a web interface for Git
- Adds collaboration features (pull requests, issues, projects)
- One of many Git hosting platforms (others: GitLab, Bitbucket, Gitea)

**Analogy:**
- **Git** = Microsoft Word (the software)
- **GitHub** = Google Docs (online platform with collaboration features)

**Key point**: You can use Git without GitHub, but you can't use GitHub without Git.

**Commands comparison:**
```bash
# Git (local operations)
git init
git add .
git commit -m "message"

# GitHub (remote operations)
git push origin main
git pull origin main
```

---

## 2. **Explain the Git workflow: Working Directory, Staging Area, and Repository**

**Explanation:**

Git has three main areas where your files exist:

**1. Working Directory (Workspace):**
- Your actual project files on your computer
- Where you edit and create files
- Untracked changes live here

**2. Staging Area (Index):**
- A preparation area before committing
- You select which changes to include in the next commit
- Gives you control over what gets committed

**3. Repository (HEAD):**
- The `.git` folder containing all committed history
- Permanent record of your project's snapshots

**The workflow:**
```bash
# 1. Modify files in Working Directory
echo "new feature" >> app.js

# 2. Stage the changes
git add app.js

# 3. Commit to Repository
git commit -m "Add new feature"

# 4. Push to remote (GitHub)
git push origin main
```

**Visual representation:**
```
Working Directory → git add → Staging Area → git commit → Repository → git push → Remote (GitHub)
```

**Why staging area exists:**
You might change 5 files but only want to commit 3 of them. Staging lets you be selective.

**Check status:**
```bash
git status  # Shows which area your changes are in
```

---

## 3. **What is a Git branch and why would you use branching?**

**Explanation:**

A branch is an independent line of development. Think of it as a parallel universe where you can make changes without affecting the main codebase.

**The main branch:**
- Usually called `main` or `master`
- Represents the production-ready code
- Should always be stable

**Why use branches:**

1. **Feature development**: Build new features without breaking main code
2. **Bug fixes**: Fix issues in isolation
3. **Experimentation**: Try things without risk
4. **Parallel work**: Multiple developers work simultaneously
5. **Code review**: Submit changes for review before merging

**Common commands:**
```bash
# Create and switch to new branch
git checkout -b feature/user-authentication

# List all branches
git branch

# Switch between branches
git checkout main

# Delete a branch
git branch -d feature/old-feature

# Push branch to GitHub
git push origin feature/user-authentication
```

**Typical workflow:**
```
main (production)
  └── feature/login-page (your work)
      ├── commit 1: Create login form
      ├── commit 2: Add validation
      └── commit 3: Style the page
  
→ Merge back to main when ready
```

**Best practices:**
- Create a branch for each feature/bug fix
- Use descriptive names: `feature/add-payment`, `bugfix/fix-navbar`, `hotfix/security-patch`
- Keep branches short-lived
- Delete branches after merging

---

## 4. **What is a Pull Request (PR) and how does it work?**

**Explanation:**

A Pull Request (called Merge Request in GitLab) is a GitHub feature that lets you propose changes to a repository and request that someone review and merge them.

**The PR process:**

**1. Developer creates a branch and makes changes:**
```bash
git checkout -b feature/add-search
# Make changes
git add .
git commit -m "Add search functionality"
git push origin feature/add-search
```

**2. Open a Pull Request on GitHub:**
- Navigate to the repository on GitHub
- Click "New Pull Request"
- Select: base branch (main) ← compare branch (feature/add-search)
- Add title and description
- Assign reviewers

**3. Code Review:**
- Reviewers examine the code
- Leave comments and suggestions
- Request changes if needed
- Approve when satisfied

**4. Merge:**
- Once approved, merge the PR
- Delete the feature branch
- Code is now in main branch

**PR best practices:**

**Good PR description template:**
```markdown
## What does this PR do?
Adds search functionality to the user dashboard

## Why is this needed?
Users requested ability to search through their orders

## How to test?
1. Go to dashboard
2. Enter search term
3. Verify results filter correctly

## Screenshots
[Add screenshots if UI changes]

## Related Issues
Closes #123
```

**Benefits:**
- Code review before merging
- Discussion and collaboration
- Continuous Integration (CI) runs tests automatically
- Documentation of why changes were made
- Prevents bad code from reaching production

**Types of merges:**
- **Merge commit**: Keeps all commits, creates merge commit
- **Squash and merge**: Combines all commits into one
- **Rebase and merge**: Replays commits on top of base branch

---

## 5. **Explain the difference between `git merge` and `git rebase`**

**Explanation:**

Both combine changes from different branches, but they do it differently.

**Git Merge:**
Creates a new "merge commit" that ties together two branches.

```bash
git checkout main
git merge feature/login

# Creates: main → merge commit → combined history
```

**Visual:**
```
Before:
main:     A → B → C
               ↓
feature:        D → E

After merge:
main:     A → B → C → M (merge commit)
               ↓     ↗
feature:        D → E
```

**Pros:**
- Preserves complete history
- Safe and familiar
- Non-destructive
- Shows when branches were merged

**Cons:**
- Creates extra merge commits
- Can make history messy with many branches

---

**Git Rebase:**
Moves your branch's commits to the tip of another branch, rewriting history.

```bash
git checkout feature/login
git rebase main

# Replays feature commits on top of main
```

**Visual:**
```
Before:
main:     A → B → C
               ↓
feature:        D → E

After rebase:
main:     A → B → C
                   ↓
feature:            D' → E' (new commits)
```

**Pros:**
- Clean, linear history
- Easier to follow
- No merge commits

**Cons:**
- Rewrites history (changes commit hashes)
- Dangerous if done on public/shared branches
- Can be confusing for beginners

**Golden Rule:**
**Never rebase public branches!** Only rebase your own local feature branches.

**When to use each:**

**Use merge:**
- Merging feature branches to main
- Working on shared branches
- Want to preserve exact history

**Use rebase:**
- Updating your feature branch with latest main
- Cleaning up local commits before PR
- Want clean, linear history

**Example workflow:**
```bash
# Keep your feature branch updated
git checkout feature/my-work
git rebase main  # Puts your work on top of latest main

# When ready to merge
git checkout main
git merge feature/my-work  # Merge to main
```

---

## 6. **What is a merge conflict and how do you resolve it?**

**Explanation:**

A merge conflict occurs when Git can't automatically merge changes because two branches modified the same part of a file differently.

**When conflicts happen:**
- Two people edit the same line in a file
- One person deletes a file while another modifies it
- Multiple branches change the same code section

**Example scenario:**
```
Developer A (on main):
function greet() {
  return "Hello World";
}

Developer B (on feature branch):
function greet() {
  return "Hi there";
}
```

When merging, Git doesn't know which version to keep.

**What a conflict looks like:**
```javascript
function greet() {
<<<<<<< HEAD (current branch)
  return "Hello World";
=======
  return "Hi there";
>>>>>>> feature/greeting-update
}
```

**Conflict markers explained:**
- `<<<<<<< HEAD`: Your current branch's version
- `=======`: Separator
- `>>>>>>> branch-name`: The incoming branch's version

**Resolution steps:**

**1. Identify the conflict:**
```bash
git status
# Shows: both modified: src/app.js
```

**2. Open the file and decide what to keep:**

**Option A - Keep current branch:**
```javascript
function greet() {
  return "Hello World";
}
```

**Option B - Keep incoming branch:**
```javascript
function greet() {
  return "Hi there";
}
```

**Option C - Keep both/create new solution:**
```javascript
function greet(formal = false) {
  return formal ? "Hello World" : "Hi there";
}
```

**3. Remove conflict markers and save:**
```bash
# After manually editing
git add src/app.js
git commit -m "Resolve merge conflict in greet function"
```

**4. Continue the merge:**
```bash
git merge --continue
# Or if rebasing:
git rebase --continue
```

**Prevention strategies:**
- Pull/sync frequently
- Communicate with team about who's working on what
- Keep feature branches short-lived
- Use smaller, focused commits
- Break large files into smaller modules

**Abort if needed:**
```bash
git merge --abort   # Cancel the merge
git rebase --abort  # Cancel the rebase
```

---

## 7. **Explain Git tags and when to use them**

**Explanation:**

Tags are pointers to specific commits, typically used to mark release versions. Unlike branches, tags don't change—they permanently mark a point in history.

**Two types of tags:**

**1. Lightweight tags:**
Just a pointer to a commit (like a bookmark).
```bash
git tag v1.0.0
```

**2. Annotated tags:**
Full objects with tagger info, date, and message (recommended).
```bash
git tag -a v1.0.0 -m "Release version 1.0.0 - Initial production release"
```

**Common use cases:**

**Version releases:**
```bash
git tag -a v2.3.1 -m "Bug fix release"
git push origin v2.3.1
```

**Marking important milestones:**
```bash
git tag -a production-2024-01-15 -m "Production deployment Jan 15"
```

**Semantic Versioning (SemVer):**
```
v1.0.0 → v1.0.1 (patch: bug fixes)
v1.0.0 → v1.1.0 (minor: new features, backward compatible)
v1.0.0 → v2.0.0 (major: breaking changes)
```

**Useful commands:**
```bash
# List all tags
git tag

# Show tag details
git show v1.0.0

# Checkout a tag (creates detached HEAD)
git checkout v1.0.0

# Create tag on specific commit
git tag -a v1.0.0 9fceb02 -m "Version 1.0"

# Push tags to remote
git push origin v1.0.0        # Push specific tag
git push origin --tags        # Push all tags

# Delete tag
git tag -d v1.0.0             # Local
git push origin --delete v1.0.0  # Remote
```

**GitHub Releases:**
Tags on GitHub can be converted to "Releases" with:
- Release notes
- Downloadable artifacts (binaries, packages)
- Change logs

**Best practices:**
- Use annotated tags for releases
- Follow semantic versioning
- Never move or change tags once pushed
- Tag stable, tested code only
- Include meaningful messages

---

## 8. **What is .gitignore and how does it work?**

**Explanation:**

`.gitignore` is a file that tells Git which files or directories to ignore and not track. This prevents unnecessary or sensitive files from being committed.

**Why use .gitignore:**
- Exclude dependencies (node_modules, vendor)
- Ignore build outputs (dist, build, *.jar)
- Hide sensitive data (.env, config files with secrets)
- Skip OS files (.DS_Store, Thumbs.db)
- Ignore IDE files (.vscode, .idea)
- Exclude logs and temporary files (*.log, *.tmp)

**Example .gitignore file:**
```gitignore
# Dependencies
node_modules/
vendor/

# Environment variables
.env
.env.local
*.env

# Build outputs
dist/
build/
*.jar
*.war

# IDE files
.vscode/
.idea/
*.swp
*.swo

# OS files
.DS_Store
Thumbs.db

# Logs
*.log
logs/

# Testing
coverage/
.pytest_cache/

# Compiled files
*.pyc
__pycache__/
*.class
```

**Pattern rules:**

```gitignore
# Ignore specific file
config.json

# Ignore all files with extension
*.log

# Ignore directory
node_modules/

# Ignore files in any subdirectory
**/temp.txt

# Exception (don't ignore)
!important.log

# Ignore files only in root
/root-only.txt

# Ignore everything in directory except one file
folder/*
!folder/keep-this.txt
```

**Common commands:**

```bash
# If you already committed files you want to ignore
git rm --cached filename.txt
git rm -r --cached node_modules/

# Then commit the removal
git commit -m "Remove ignored files from tracking"
```

**Global .gitignore:**
For personal preferences across all projects:
```bash
# Create global gitignore
git config --global core.excludesfile ~/.gitignore_global

# Add to ~/.gitignore_global
.DS_Store
.vscode/
```

**Templates:**
GitHub provides templates for different languages:
- https://github.com/github/gitignore

**Important notes:**
- `.gitignore` only affects untracked files
- Files already tracked must be manually removed
- Place `.gitignore` in repository root (or subdirectories for local rules)
- Commit `.gitignore` to repository so team shares same rules

**Check if file is ignored:**
```bash
git check-ignore -v filename.txt
```

---

## 9. **What is Git stash and when would you use it?**

**Explanation:**

`git stash` temporarily saves your uncommitted changes so you can work on something else, then reapply them later. It's like putting your work in a drawer while you handle an urgent task.

**Common scenarios:**

**Scenario 1: Urgent bug fix needed**
```bash
# You're working on a feature
# Boss: "Emergency! Fix production bug NOW!"

git stash                    # Save current work
git checkout main
git checkout -b hotfix/bug

# Fix the bug
git add .
git commit -m "Fix critical bug"

git checkout feature/my-work
git stash pop               # Resume your work
```

**Scenario 2: Wrong branch**
```bash
# Oops! Made changes on main instead of feature branch
git stash
git checkout -b feature/correct-branch
git stash pop               # Apply changes to correct branch
```

**Scenario 3: Pull latest changes**
```bash
# Need to pull but have uncommitted changes
git stash
git pull origin main
git stash pop               # Reapply your changes on top
```

**Basic commands:**

```bash
# Stash current changes
git stash

# Stash with descriptive message
git stash save "WIP: implementing login form"

# List all stashes
git stash list
# Output:
# stash@{0}: WIP on feature: implementing login form
# stash@{1}: WIP on main: fixing navbar

# Apply most recent stash and remove it
git stash pop

# Apply most recent stash but keep it
git stash apply

# Apply specific stash
git stash apply stash@{1}

# Delete stash
git stash drop stash@{0}

# Delete all stashes
git stash clear

# Show what's in a stash
git stash show -p stash@{0}
```

**Advanced options:**

```bash
# Stash including untracked files
git stash -u

# Stash only unstaged changes (keep staged)
git stash --keep-index

# Create branch from stash
git stash branch feature/new-work stash@{0}
```

**Stash vs Commit:**

| Stash | Commit |
|-------|--------|
| Temporary | Permanent |
| Not shared with team | Shared via push |
| Quick save | Requires message |
| For work in progress | For complete changes |

**Best practices:**
- Use descriptive messages with `git stash save "message"`
- Don't keep stashes for too long
- Pop/apply stashes regularly
- Prefer commits over stashes when possible
- Clean old stashes periodically

**Handling conflicts:**
If stash pop causes conflicts:
```bash
# Fix conflicts manually
git add .
git stash drop  # Remove applied stash
```

---

## 10. **Explain Git reset, revert, and checkout - what are the differences?**

**Explanation:**

These three commands all "undo" things but work very differently. Understanding them is crucial.

---

### **Git Reset** (Rewind history)

Moves the branch pointer backwards, potentially losing commits.

**Three modes:**

**1. Soft reset** (keep changes staged):
```bash
git reset --soft HEAD~1

# Undoes last commit
# Changes stay staged
# Use when: You want to recommit with better message
```

**2. Mixed reset** (default - keep changes unstaged):
```bash
git reset HEAD~1
# or
git reset --mixed HEAD~1

# Undoes last commit
# Changes become unstaged
# Use when: You want to re-organize what gets committed
```

**3. Hard reset** (discard everything):
```bash
git reset --hard HEAD~1

# Undoes last commit
# DELETES all changes
# Use when: You want to completely abandon changes
# ⚠️ DANGEROUS - can't recover deleted changes
```

**Examples:**
```bash
# Undo last 3 commits
git reset --soft HEAD~3

# Reset to specific commit
git reset --hard abc1234

# Unstage a file
git reset HEAD file.txt
```

---

### **Git Revert** (Create new commit that undoes)

Creates a new commit that reverses previous changes. Doesn't rewrite history.

```bash
git revert HEAD

# Creates new commit that undoes the last commit
# Safe for public/shared branches
# History is preserved
```

**Why use revert:**
- Safe for public branches
- Maintains audit trail
- Can revert old commits without affecting newer ones

**Example:**
```bash
# Revert specific commit
git revert abc1234

# Revert without auto-commit (review first)
git revert -n abc1234
git commit -m "Revert feature X due to bug"
```

---

### **Git Checkout** (Switch branches or restore files)

Has two main uses:

**1. Switch branches:**
```bash
git checkout main
git checkout -b feature/new
```

**2. Restore specific files:**
```bash
# Discard changes to file (restore from last commit)
git checkout -- filename.txt

# Restore file from specific commit
git checkout abc1234 -- filename.txt
```

**Note:** In newer Git (2.23+), checkout is split:
```bash
# Switch branches
git switch main
git switch -c feature/new

# Restore files
git restore filename.txt
git restore --source=abc1234 filename.txt
```

---

### **Comparison Table:**

| Command | What it does | Rewrites history? | Safe for public branches? |
|---------|--------------|-------------------|---------------------------|
| `reset --soft` | Undo commits, keep changes staged | Yes | No |
| `reset --mixed` | Undo commits, unstage changes | Yes | No |
| `reset --hard` | Undo commits, delete changes | Yes | No |
| `revert` | Create new commit undoing changes | No | Yes |
| `checkout` | Switch branches or restore files | No | Yes |

---

### **Decision Tree:**

```
Need to undo changes?
│
├─ On public/shared branch?
│  └─ Yes → Use REVERT
│
└─ On private/local branch?
   │
   ├─ Want to keep changes?
   │  ├─ Staged → reset --soft
   │  └─ Unstaged → reset --mixed
   │
   └─ Want to delete changes?
      └─ reset --hard (careful!)
```

---

### **Real-world examples:**

**Scenario 1: Oops, bad commit message**
```bash
git reset --soft HEAD~1
git commit -m "Better commit message"
```

**Scenario 2: Pushed bug to production**
```bash
git revert HEAD
git push origin main  # Safe for production
```

**Scenario 3: Experimental changes failed**
```bash
git reset --hard HEAD  # Throw it all away
```

**Scenario 4: Accidentally modified file**
```bash
git checkout -- config.js  # Restore file
```

---

## 11. **What is Git cherry-pick and when would you use it?**

**Explanation:**

`git cherry-pick` applies a specific commit from one branch to another. It's like copy-pasting a commit to a different branch.

**When to use cherry-pick:**

**Scenario 1: Urgent bug fix needed in multiple branches**
```bash
# Fixed bug on main
git checkout main
git commit -m "Fix critical security bug" # Creates commit abc1234

# Now need same fix on release branch
git checkout release-2.0
git cherry-pick abc1234  # Applies the same fix
```

**Scenario 2: Committed to wrong branch**
```bash
# Oops! Committed feature to main instead of feature branch
git checkout feature/my-work
git cherry-pick abc1234  # Copy commit to correct branch

git checkout main
git reset --hard HEAD~1  # Remove from wrong branch
```

**Scenario 3: Want only specific commits from a feature**
```bash
# Feature branch has 10 commits but you only want commit 5
git checkout main
git cherry-pick def5678  # Just take that one commit
```

**Basic commands:**

```bash
# Cherry-pick single commit
git cherry-pick abc1234

# Cherry-pick multiple commits
git cherry-pick abc1234 def5678 ghi9012

# Cherry-pick range of commits
git cherry-pick abc1234..def5678

# Cherry-pick without auto-commit (review first)
git cherry-pick -n abc1234
# Review changes
git commit -m "Cherry-picked feature X"
```

**Handling conflicts:**
```bash
git cherry-pick abc1234
# CONFLICT! Fix manually
git add .
git cherry-pick --continue

# Or abort
git cherry-pick --abort
```

**Advanced options:**

```bash
# Cherry-pick and edit commit message
git cherry-pick -e abc1234

# Cherry-pick with different author
git cherry-pick abc1234 --edit

# Sign-off cherry-pick
git cherry-pick -s abc1234
```

**Real-world workflow:**

```bash
# Production hotfix scenario
git checkout production
git checkout -b hotfix/security-patch

# Make fix
git commit -m "Patch security vulnerability"  # commit abc1234

# Apply to main
git checkout main
git cherry-pick abc1234

# Apply to develop
git checkout develop
git cherry-pick abc1234

# Apply to release branch
git checkout release-3.0
git cherry-pick abc1234
```

**Pros:**
- Selective commit application
- Quick fixes across branches
- Doesn't merge entire branch

**Cons:**
- Creates duplicate commits (different hashes)
- Can make history confusing
- Conflicts can be tricky

**Best practices:**
- Use sparingly (prefer merging when possible)
- Document why you cherry-picked
- Don't cherry-pick from public to private branches repeatedly
- Consider merge if you need many commits

**Cherry-pick vs Merge:**

| Cherry-pick | Merge |
|-------------|-------|
| Single/few commits | All commits |
| Duplicates commits | Maintains single history |
| Selective | All-or-nothing |
| Can cause confusion | Clear branch integration |

---

## 12. **Explain Git hooks and give examples of their use cases**

**Explanation:**

Git hooks are scripts that automatically run at certain points in the Git workflow. They let you automate tasks, enforce policies, and prevent mistakes.

**Where hooks live:**
```
.git/hooks/
├── pre-commit.sample
├── pre-push.sample
├── commit-msg.sample
└── ... (rename to remove .sample to activate)
```

**Types of hooks:**

### **Client-side hooks** (on your machine):

**1. pre-commit** (runs before commit is created)
```bash
#!/bin/bash
# .git/hooks/pre-commit

# Run linting
npm run lint
if [ $? -ne 0 ]; then
  echo "❌ Linting failed. Fix errors before committing."
  exit 1
fi

# Run tests
npm test
if [ $? -ne 0 ]; then
  echo "❌ Tests failed. Fix tests before committing."
  exit 1
fi

echo "✅ Pre-commit checks passed!"
```

**Use cases:**
- Run linters (ESLint, Prettier)
- Run tests
- Check for debugging code (console.log, debugger)
- Verify code formatting
- Check for large files

**2. commit-msg** (validates commit message)
```bash
#!/bin/bash
# .git/hooks/commit-msg

commit_msg=$(cat "$1")

# Enforce commit message format: "TYPE: message"
if ! echo "$commit_msg" | grep -qE "^(feat|fix|docs|style|refactor|test|chore): .+"; then
  echo "❌ Invalid commit message format!"
  echo "Must be: TYPE: description"
  echo "Types: feat, fix, docs, style, refactor, test, chore"
  echo "Example: feat: add user authentication"
  exit 1
fi

echo "✅ Commit message valid!"
```

**Use cases:**
- Enforce conventional commits
- Require issue numbers
- Check message length
- Validate format

**3. pre-push** (runs before push)
```bash
#!/bin/bash
# .git/hooks/pre-push

echo "Running tests before push..."
npm test

if [ $? -ne 0 ]; then
  echo "❌ Tests failed. Push aborted."
  exit 1
fi

echo "✅ Tests passed. Pushing..."
```

**Use cases:**
- Run full test suite
- Check branch protection
- Verify no sensitive data
- Ensure build succeeds

**4. post-merge** (runs after successful merge)
```bash
#!/bin/bash
# .git/hooks/post-merge

# Check if package.json changed
if git diff-tree -r --name-only --no-commit-id ORIG_HEAD HEAD | grep --quiet "package.json"; then
  echo "📦 package.json changed. Running npm install..."
  npm install
fi
```

**Use cases:**
- Auto-install dependencies
- Rebuild after merge
- Update environment

---

### **Server-side hooks** (on GitHub/GitLab/remote):

**1. pre-receive** (before push is accepted)
- Enforce branch naming
- Reject force pushes to main
- Check commit messages

**2. post-receive** (after push is accepted)
- Trigger CI/CD pipelines
- Send notifications
- Deploy code

---

### **Popular hook managers:**

**Husky** (most popular for JavaScript projects):
```bash
npm install --save-dev husky

# Initialize
npx husky init

# Add pre-commit hook
echo "npm test" > .husky/pre-commit
```

**package.json setup:**
```json
{
  "scripts": {
    "prepare": "husky"
  },
  "devDependencies": {
    "husky": "^9.0.0",
    "lint-staged": "^15.0.0"
  },
  "lint-staged": {
    "*.js": ["eslint --fix", "prettier --write"]
  }
}
```

**pre-commit framework** (Python):
```yaml
# .pre-commit-config.yaml
repos:
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v4.5.0
    hooks:
      - id: trailing-whitespace
      - id: end-of-file-fixer
      - id: check-yaml
      - id: check-added-large-files
```

---

### **Real-world examples:**

**Prevent committing secrets:**
```bash
#!/bin/bash
# .git/hooks/pre-commit

if git diff --cached | grep -E '(API_KEY|PASSWORD|SECRET).*=.*[A-Za-z0-9]+'; then
  echo "❌ Potential secret detected in commit!"
  echo "Remove sensitive data before committing."
  exit 1
fi
```

**Enforce branch naming:**
```bash
#!/bin/bash
# .git/hooks/pre-push

branch=$(git rev-parse --abbrev-ref HEAD)

if ! echo "$branch" | grep -qE "^(feature|bugfix|hotfix|release)/[a-z0-9-]+$"; then
  echo "❌ Invalid branch name: $branch"
  echo "Use: feature/*, bugfix/*, hotfix/*, or release/*"
  exit 1
fi
```

**Auto-format code:**
```bash
#!/bin/bash
# .git/hooks/pre-commit

echo "🎨 Auto-formatting code..."

# Format staged files
git diff --cached --name-only --diff-filter=ACMR | grep -E '\.(js|jsx|ts|tsx)$' | xargs npx prettier --write

# Re-add formatted files
git add -u

echo "✅ Code formatted!"
```

---

### **Best practices:**

1. **Make hooks fast** - slow hooks frustrate developers
2. **Allow bypass** - sometimes you need to skip checks:
   ```bash
   git commit --no-verify
   ```
3. **Share with team** - commit hooks to repo using tools like Husky
4. **Clear error messages** - tell developers how to fix issues
5. **Don't break workflow** - hooks should help, not hinder

---

## 13. **What is Git rebase -i (interactive rebase) and when to use it?**

**Explanation:**

Interactive rebase (`git rebase -i`) is a powerful tool that lets you clean up, reorganize, and edit your commit history before sharing it with others.

**When to use interactive rebase:**
- Clean up messy commit history
- Combine multiple small commits into one
- Split a large commit into smaller ones
- Reorder commits
- Edit commit messages
- Remove commits entirely

**⚠️ Golden rule: Only use on commits that haven't been pushed to shared branches!**

---

### **Basic usage:**

```bash
# Rebase last 3 commits
git rebase -i HEAD~3

# Rebase from specific commit
git rebase -i abc1234
```

**This opens an editor showing:**
```
pick abc1234 Add user model
pick def5678 Fix typo
pick ghi9012 Add validation
pick jkl3456 Fix validation bug
pick mno7890 More validation fixes

# Commands:
# p, pick = use commit
# r, reword = use commit, but edit message
# e, edit = use commit, but stop for amending
# s, squash = use commit, merge into previous commit
# f, fixup = like squash, discard this commit's message
# d, drop = remove commit
```

---

### **Common operations:**

**1. Squash commits** (combine multiple commits):
```
pick abc1234 Add user model
squash def5678 Fix typo
squash ghi9012 Add more fields
```
Result: One commit with message from all three

**2. Fixup commits** (squash but keep only first message):
```
pick abc1234 Add user authentication
fixup def5678 Fix linting issues
fixup ghi9012 Fix typo in auth
```
Result: One clean commit "Add user authentication

```
Result: One clean commit "Add user authentication"

---

**3. Reword commit messages:**
```
pick abc1234 Add user model
reword def5678 Fix typo
pick ghi9012 Add validation
```
Git will pause to let you edit the "Fix typo" message

---

**4. Reorder commits:**
```
pick ghi9012 Add validation      # Move this up
pick abc1234 Add user model
pick def5678 Fix typo
```

---

**5. Edit a commit** (to add forgotten files):
```
pick abc1234 Add user model
edit def5678 Add authentication   # Stop here
pick ghi9012 Add tests
```

When Git pauses:
```bash
# Make your changes
git add forgotten-file.js
git commit --amend --no-edit
git rebase --continue
```

---

**6. Drop commits:**
```
pick abc1234 Add user model
drop def5678 Debug code          # Remove this
pick ghi9012 Add validation
```

---

### **Real-world workflow examples:**

**Scenario 1: Clean up before Pull Request**
```bash
# Your messy history:
# - Add login feature
# - Fix typo
# - Actually fix the typo
# - Add validation
# - Fix validation
# - Remove console.log
# - Fix lint errors

git rebase -i HEAD~7

# Clean it up to:
pick abc1234 Add login feature with validation
# (squashed all related commits)
```

**Result: Clean, professional commit history for code review**

---

**Scenario 2: Fix commit in the middle**
```bash
git log --oneline
# abc1234 Add tests
# def5678 Add authentication (forgot to add a file!)
# ghi9012 Add user model

git rebase -i HEAD~3

# In editor:
edit def5678 Add authentication
pick abc1234 Add tests

# Git pauses at def5678
git add forgotten-file.js
git commit --amend --no-edit
git rebase --continue
```

---

**Scenario 3: Split one large commit into multiple**
```bash
git rebase -i HEAD~1

# In editor:
edit abc1234 Added multiple features

# Git pauses
git reset HEAD~1              # Unstage everything
git add user-model.js
git commit -m "Add user model"

git add authentication.js
git commit -m "Add authentication"

git add validation.js
git commit -m "Add validation"

git rebase --continue
```

---

### **Interactive rebase shortcuts:**

**Autosquash workflow:**
```bash
# Make a commit
git commit -m "Add feature"

# Oops, found a bug in that feature
git add bugfix.js
git commit --fixup abc1234    # Creates "fixup! Add feature"

# Later, clean up
git rebase -i --autosquash HEAD~5
# Automatically arranges fixup commits next to originals
```

**Auto-squash config:**
```bash
git config --global rebase.autoSquash true
# Now git rebase -i always uses autosquash
```

---

### **Handling conflicts during interactive rebase:**

```bash
git rebase -i HEAD~5

# CONFLICT appears
# Fix conflicts in files
git add resolved-file.js

# Continue rebase
git rebase --continue

# Or skip problematic commit
git rebase --skip

# Or abort and start over
git rebase --abort
```

---

### **Advanced techniques:**

**1. Exec command** (run command after each commit):
```
pick abc1234 Add feature A
exec npm test
pick def5678 Add feature B
exec npm test
```
Tests run after each commit; aborts if tests fail

**2. Break** (pause for manual intervention):
```
pick abc1234 Add feature A
break
pick def5678 Add feature B
```

**3. Update-ref** (update branch pointers):
```
pick abc1234 Commit 1
update-ref refs/heads/feature-branch
pick def5678 Commit 2
```

---

### **Common mistakes and how to fix them:**

**Mistake 1: Rebased pushed commits**
```bash
# Other developers have your commits
git push --force-with-lease   # Safer than --force
# But communicate with team first!
```

**Mistake 2: Rebase went wrong**
```bash
# Git saves your original branch
git reflog
# Find your previous HEAD
git reset --hard HEAD@{5}
```

**Mistake 3: Lost commits during rebase**
```bash
git reflog
# Shows all HEAD movements
git cherry-pick abc1234   # Recover lost commit
```

---

### **Best practices:**

1. **Only rebase unpushed commits**
   ```bash
   # Safe
   git rebase -i origin/main
   
   # Dangerous if others have these commits
   git rebase -i HEAD~10
   ```

2. **Use descriptive commit messages**
   - Easier to identify what to squash/reorder

3. **Rebase frequently**
   - Small, frequent cleanups easier than big ones

4. **Test after rebasing**
   ```bash
   git rebase -i HEAD~5
   npm test    # Make sure nothing broke
   ```

5. **Keep a backup branch**
   ```bash
   git branch backup-before-rebase
   git rebase -i HEAD~10
   # If disaster: git reset --hard backup-before-rebase
   ```

6. **Use --autosquash workflow**
   - More organized than manual squashing

---

### **Interactive rebase cheat sheet:**

| Command | Shortcut | What it does |
|---------|----------|--------------|
| pick | p | Keep commit as-is |
| reword | r | Keep commit, change message |
| edit | e | Keep commit, pause to amend |
| squash | s | Merge with previous, keep both messages |
| fixup | f | Merge with previous, discard message |
| drop | d | Delete commit |
| exec | x | Run shell command |
| break | b | Pause rebase |

---

### **When NOT to use interactive rebase:**

❌ On public/shared branches (main, develop)
❌ On commits others have based work on
❌ If you're uncomfortable with Git recovery
❌ Right before a critical deadline (risky time to experiment)

✅ On feature branches before merging
✅ On local commits not yet pushed
✅ When preparing clean history for code review
✅ When you understand the basics of Git

---

## 14. **What are Git submodules and when would you use them?**

**Explanation:**

Git submodules allow you to include one Git repository inside another as a subdirectory. The submodule maintains its own commit history and remote repository.

**Use cases:**
- Shared libraries across multiple projects
- Third-party dependencies tracked from source
- Separating large monorepos
- Plugin architectures
- Vendor code that needs version tracking

---

### **How submodules work:**

**Project structure:**
```
my-project/
├── .git/
├── .gitmodules          # Tracks submodule info
├── src/
└── shared-library/      # Submodule (separate repo)
    └── .git             # Points to actual repo
```

---

### **Basic submodule operations:**

**1. Adding a submodule:**
```bash
# Add submodule
git submodule add https://github.com/user/shared-library.git libs/shared

# This creates:
# - .gitmodules file
# - libs/shared/ directory
# - Entry in .git/config

git commit -m "Add shared library submodule"
git push
```

**The .gitmodules file:**
```ini
[submodule "libs/shared"]
    path = libs/shared
    url = https://github.com/user/shared-library.git
    branch = main
```

---

**2. Cloning a repo with submodules:**

```bash
# Method 1: Clone and initialize submodules
git clone https://github.com/user/my-project.git
cd my-project
git submodule init
git submodule update

# Method 2: Clone with submodules in one command
git clone --recursive https://github.com/user/my-project.git

# Method 3: After cloning without --recursive
git submodule update --init --recursive
```

---

**3. Updating submodules:**

```bash
# Update to latest commit from submodule's remote
cd libs/shared
git pull origin main
cd ../..
git add libs/shared
git commit -m "Update shared library to latest"

# Or update all submodules from parent
git submodule update --remote

# Update and merge (if you have local changes)
git submodule update --remote --merge
```

---

**4. Working inside submodules:**

```bash
# Submodules are in "detached HEAD" state by default
cd libs/shared
git checkout main           # Switch to branch
# Make changes
git add .
git commit -m "Update library"
git push origin main

# Back to parent repo
cd ../..
git add libs/shared         # Record new commit reference
git commit -m "Update submodule reference"
```

---

**5. Removing a submodule:**

```bash
# Deinitialize
git submodule deinit libs/shared

# Remove from .git/modules
rm -rf .git/modules/libs/shared

# Remove directory
git rm -f libs/shared

# Commit removal
git commit -m "Remove shared library submodule"
```

---

### **Real-world examples:**

**Example 1: Shared UI component library**
```bash
# Main app
my-app/
├── src/
└── libs/
    └── ui-components/      # Submodule

# Multiple apps use same UI library
app-1/ → libs/ui-components (submodule)
app-2/ → libs/ui-components (submodule)
app-3/ → libs/ui-components (submodule)
```

**Example 2: Documentation site**
```bash
docs-site/
├── content/
└── themes/
    └── hugo-theme/         # Submodule to theme repo
```

**Example 3: Microservices**
```bash
infrastructure/
├── terraform/
└── services/
    ├── api-service/        # Submodule
    ├── auth-service/       # Submodule
    └── data-service/       # Submodule
```

---

### **Common workflows:**

**Updating all submodules to latest:**
```bash
git submodule update --remote --recursive
git add .
git commit -m "Update all submodules to latest"
```

**Checking submodule status:**
```bash
git submodule status

# Output shows:
# +abc1234 libs/shared (heads/main)
# - = not initialized
# + = different commit than recorded
#   = matches recorded commit
```

**Running commands in all submodules:**
```bash
git submodule foreach 'git pull origin main'
git submodule foreach 'git checkout main'
git submodule foreach 'npm install'
```

**Lock submodule to specific commit:**
```bash
cd libs/shared
git checkout abc1234    # Specific commit
cd ../..
git add libs/shared
git commit -m "Lock shared library to v2.1.0"
```

---

### **Common issues and solutions:**

**Issue 1: Submodule directory empty after clone**
```bash
# Solution:
git submodule update --init --recursive
```

**Issue 2: Submodule in detached HEAD state**
```bash
# Solution: checkout a branch before making changes
cd libs/shared
git checkout main
```

**Issue 3: Conflicts when updating parent repo**
```bash
# Solution:
git submodule update --remote --merge
# Or manually resolve in submodule
cd libs/shared
git merge origin/main
```

**Issue 4: Team members forget to update submodules**
```bash
# Solution: Add git hook
# .git/hooks/post-merge
#!/bin/bash
git submodule update --init --recursive
```

---

### **Submodules vs Alternatives:**

| Approach | Pros | Cons | Use When |
|----------|------|------|----------|
| **Submodules** | Precise version control, independent repos | Complex, easy to mess up | Need separate versioning |
| **Git Subtree** | Simpler, part of main repo | Entire history copied | Want unified history |
| **Package Manager** (npm, pip) | Standard tool, well understood | Less control, published packages only | Public dependencies |
| **Monorepo** | Single source of truth, atomic changes | Large repo size, tooling needed | Tight coupling preferred |

---

### **Best practices:**

1. **Document submodule setup clearly**
   ```markdown
   # README.md
   ## Setup
   git clone --recursive https://github.com/user/project.git
   # Or if already cloned:
   git submodule update --init --recursive
   ```

2. **Use branch tracking**
   ```bash
   git submodule add -b main https://github.com/user/lib.git libs/lib
   # Tracks main branch, not just commit
   ```

3. **Automate submodule updates in CI/CD**
   ```yaml
   # .github/workflows/ci.yml
   - name: Checkout with submodules
     uses: actions/checkout@v3
     with:
       submodules: recursive
   ```

4. **Consider alternatives first**
   - Use package managers when possible
   - Submodules add complexity

5. **Pin to tags/releases, not branches**
   ```bash
   cd libs/shared
   git checkout v2.1.0  # Tag
   cd ../..
   git add libs/shared
   git commit -m "Pin to stable release"
   ```

6. **Create wrapper scripts**
   ```bash
   # scripts/update-submodules.sh
   #!/bin/bash
   git submodule update --init --recursive
   git submodule foreach 'git checkout main && git pull'
   ```

---

### **When to avoid submodules:**

❌ Team unfamiliar with Git
❌ Tight coupling between repos
❌ Frequent cross-repo changes
❌ Public packages available via package managers
❌ Want simpler workflow

✅ Independent versioning needed
✅ Shared code across many projects
✅ Vendor/third-party source tracking
✅ Team comfortable with Git complexity

---

## 15. **What is Git flow and explain branching strategies (Git Flow, GitHub Flow, GitLab Flow)**

**Explanation:**

Branching strategies define how teams organize their work across branches. They provide structure for development, testing, and releases.

---

### **Git Flow** (Traditional, comprehensive)

**Created by:** Vincent Driessen (2010)

**Branch structure:**
```
main (production)
  ├── develop (integration)
  │   ├── feature/user-auth
  │   ├── feature/payment
  │   └── feature/dashboard
  ├── release/1.2.0
  │   └── bugfix/critical-fix
  └── hotfix/security-patch
```

**Branches:**

1. **main** (or master)
   - Production-ready code
   - Only updated from release or hotfix branches
   - Tagged with version numbers

2. **develop**
   - Integration branch
   - Latest delivered development changes
   - Nightly builds run from here

3. **feature/** branches
   - Created from: develop
   - Merged into: develop
   - Naming: feature/feature-name
   ```bash
   git checkout develop
   git checkout -b feature/user-authentication
   # Work on feature
   git checkout develop
   git merge --no-ff feature/user-authentication
   git branch -d feature/user-authentication
   ```

4. **release/** branches
   - Created from: develop
   - Merged into: main AND develop
   - Naming: release/1.2.0
   ```bash
   git checkout develop
   git checkout -b release/1.2.0
   # Bug fixes only, no new features
   # When ready:
   git checkout main
   git merge --no-ff release/1.2.0
   git tag -a v1.2.0
   git checkout develop
   git merge --no-ff release/1.2.0
   git branch -d release/1.2.0
   ```

5. **hotfix/** branches
   - Created from: main
   - Merged into: main AND develop
   - Naming: hotfix/critical-bug
   ```bash
   git checkout main
   git checkout -b hotfix/security-fix
   # Fix the bug
   git checkout main
   git merge --no-ff hotfix/security-fix
   git tag -a v1.2.1
   git checkout develop
   git merge --no-ff hotfix/security-fix
   git branch -d hotfix/security-fix
   ```

**Git Flow workflow:**
```
1. Feature development:
   develop → feature/X → develop

2. Release preparation:
   develop → release/1.0 → main (tag v1.0) + develop

3. Hotfix:
   main → hotfix/fix → main (tag v1.0.1) + develop
```

**Pros:**
- Clear structure for releases
- Parallel development support
- Good for scheduled releases
- Isolated hotfix process

**Cons:**
- Complex for continuous delivery
- Many long-lived branches
- Merge conflicts more common
- Overkill for small teams

**Best for:**
- Desktop software (versioned releases)
- Mobile apps (app store submissions)
- Scheduled release cycles
- Large teams

---

### **GitHub Flow** (Simplified, continuous deployment)

**Created by:** GitHub

**Branch structure:**
```
main (production, always deployable)
  ├── feature/add-login
  ├── fix/navbar-bug
  └── update/dependencies
```

**Workflow:**

**1. Everything deploys from main**
```bash
# main is ALWAYS deployable
# Deployed continuously or on-demand
```

**2. Create descriptive branches**
```bash
git checkout main
git pull origin main
git checkout -b feature/add-payment-integration
```

**3. Commit frequently**
```bash
git add .
git commit -m "Add Stripe integration"
git push origin feature/add-payment-integration
```

**4. Open Pull Request early**
```bash
# Open PR even if not done (mark as draft)
# Get feedback early
# Continuous discussion
```

**5. Discuss and review**
- Team reviews code
- CI/CD runs automatically
- Make changes based on feedback

**6. Merge after review**
```bash
# After approval and passing tests
git checkout main
git merge --no-ff feature/add-payment-integration
git push origin main
```

**7. Deploy immediately**
```bash
# main is deployed automatically or manually
# If issues, revert and fix
```

**Pros:**
- Simple and easy to understand
- Fast feature delivery
- Continuous deployment friendly
- Fewer merge conflicts

**Cons:**
- No release branches (everything goes to production)
- Less suitable for versioned releases
- Requires strong CI/CD
- Needs excellent test coverage

**Best for:**
- Web applications
- SaaS products
- APIs and services
- Small to medium teams
- Continuous deployment

---

### **GitLab Flow** (Middle ground)

**Created by:** GitLab

**Combines Git Flow and GitHub Flow concepts**

**Three variations:**

---

**Variation 1: Production branch** (most common)
```
main (development)
  └── production (deployed)
      └── Tags: v1.0, v1.1, v1.2
```

**Workflow:**
```bash
# Feature development
git checkout main
git checkout -b feature/new-feature
# Work and commit
git push origin feature/new-feature
# Create merge request → merge to main

# When ready to deploy
git checkout production
git merge main
git push origin production
# Triggers deployment
```

---

**Variation 2: Environment branches**
```
main (development)
  ├── staging
  └── production
```

**Workflow:**
```bash
# Development
main → merge request → main

# Testing
main → staging (auto-deploy to staging environment)

# Production
staging → production (after testing passes)
```

---

**Variation 3: Release branches**
```
main (development)
  ├── release/2.0
  ├── release/2.1
  └── release/3.0
```

**Workflow:**
```bash
# For versioned releases (mobile apps, etc.)
git checkout main
git checkout -b release/2.0
# Only bug fixes go to release branch
# Cherry-pick fixes back to main if needed
```

---

**Key principles:**

1. **Upstream first**
   - Always merge to main first
   - Then cherry-pick or merge to release/production
   - Prevents fixes from being lost

2. **Commit only to main**
   - Feature branches merge to main only
   - Never commit directly to production branches

3. **Use merge requests for everything**
   - Code review required
   - CI/CD runs automatically
   - Discussion and approval process

**Pros:**
- Flexible for different needs
- Supports both continuous and versioned releases
- Clear environment progression
- Upstream-first prevents lost fixes

**Cons:**
- Can be complex with multiple environment branches
- Requires discipline
- May need cherry-picking

**Best for:**
- Enterprise applications
- Products with multiple environments
- Teams needing flexibility
- SaaS with staging environments

---

### **Comparison table:**

| Feature | Git Flow | GitHub Flow | GitLab Flow |
|---------|----------|-------------|-------------|
| **Complexity** | High | Low | Medium |
| **Branches** | 5 types | 2 types | 2-3 types |
| **Release style** | Scheduled | Continuous | Flexible |
| **Best for** | Versioned software | Web apps | Enterprise |
| **Learning curve** | Steep | Gentle | Moderate |
| **Merge conflicts** | More common | Less common | Moderate |
| **Production hotfixes** | Dedicated branch | Revert + fix | Cherry-pick or revert |

---

### **Choosing the right strategy:**

**Choose Git Flow if:**
- Desktop/mobile apps with versioned releases
- Multiple production versions supported simultaneously
- Scheduled release cycles (quarterly, monthly)
- Large team with complex release process

**Choose GitHub Flow if:**
- Web application with continuous deployment
- Single production version
- Small to medium team
- Want simplicity
- Strong automated testing

**Choose GitLab Flow if:**
- Need multiple environments (dev, staging, production)
- Want balance between structure and flexibility
- Enterprise application
- Both continuous and versioned releases needed

---

### **Modern trends:**

**Trunk-Based Development** (gaining popularity):
```
main (trunk)
  ├── short-lived feature branches (< 2 days)
  └── feature flags for incomplete features
```

**Principles:**
- Very short-lived branches
- Commit to main daily
- Use feature flags to hide incomplete work
- Continuous integration
- Fast feedback

**Benefits:**
- Minimal merge conflicts
- Faster integration
- Encourages small changes
- Supports continuous deployment

---

### **Best practices (all strategies):**

1. **Protect main/production branches**
   ```bash
   # GitHub settings:
   # Require pull request reviews
   # Require status checks to pass
   # No direct pushes
   ```

2. **Use meaningful branch names**
   ```bash
   # Good:
   feature/user-authentication
   bugfix/login-validation-error
   hotfix/security-cve-2024-1234
   
   # Bad:
   test
   fix
   my-branch
   ```

3. **Delete merged branches**
   ```bash
   git branch -d feature/completed-feature
   git push origin --delete feature/completed-feature
   ```

4. **Keep branches up to date**
   ```bash
   # Rebase or merge from main regularly
   git checkout feature/my-feature
   git rebase main
   ```

5. **Automate with CI/CD**
   - Run tests on every push
   - Deploy automatically when possible
   - Enforce branch policies

6. **Document your strategy**
   ```markdown
   # CONTRIBUTING.md
   ## Branching Strategy
   We use GitHub Flow:
   1. Create feature branch from main
   2. Make changes and commit
   3. Open Pull Request
   4. Get review and approval
   5. Merge to main
   6. Auto-deploy to production
   ```
---

