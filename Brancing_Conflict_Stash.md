--------------------
# Git Branching
--------------------
Git branches allow you to work on different features or tasks in parallel. Here's a guide on how to create and work with Git branches.

![image](https://github.com/asiandevs/DevOps-Git/assets/37457408/002782ff-a79c-412c-a9e5-31a5feabc5e9)

## Step 1: Create a New Branch

1. **Open a Terminal or Command Prompt:**
   Open your command line interface.

2. **Navigate to Your Git Repository:**
   Use the `cd` command to move to your Git repository's directory.

   ```bash
   cd /path/to/your/repo
   ```

3. **Create a New Branch:**
   To create a new branch, use the `git branch` command, followed by the branch name. For example, to create a branch named "fb," run:

   ```bash
   git branch fb
   ```

   This creates the branch, but you're not on it yet.

## Step 2: Switch to the New Branch

1. **Switch to the New Branch:**
   Use the `git checkout` command to switch to the newly created branch. For example:

   ```bash
   git checkout fb
   ```

   You are now on the "fb" and ready to start working on it.

   Alternatively, you can create and switch to a new branch in one command:

   ```bash
   git checkout -b fb
   ```

## Step 3: Make Changes

With your new branch checked out, you can start making changes to your project, such as adding new features or fixing bugs. Use your preferred text editor or integrated development environment (IDE) to make modifications.
Add two files [e.g. file1.js file2.js - in my case empty but think this is your java code]

   ```bash
   touch file1.js file2.js
   ```

## Step 4: Stage and Commit Changes

1. **Stage Your Changes:**
   
   Use the `git add` command to stage the changes you want to include in the next commit. For example:

   ```bash
   git add file1.js file2.js
   ```

3. **Commit Your Changes:**
   After staging your changes, commit them to the branch with a commit message:

   ```bash
   git commit -m "Add new feature"
   ```
4. **Checkout to the new branch and merge:**
   After staging your changes, commit them to the branch with a commit message:

   ```bash
   git commit -m "Add new feature"
   git merge fb master
   ```

-------------------------------
# Git Conflict Management
-------------------------------
Git conflicts occur when multiple contributors make conflicting changes to the same part of a file in a collaborative Git repository. Resolving these conflicts is essential to ensure the codebase remains functional. Here's a step-by-step guide on managing Git conflicts:

## Step 1: Pull the Latest Changes

Before making your own changes, it's a good practice to pull the latest changes from the remote repository to ensure you're working with the most up-to-date code:

```bash
git pull origin <branch-name>
```

## Step 2: Resolve Conflicts

When you pull changes that conflict with your own, Git will notify you about the conflicts in the affected files. Open the conflicted file(s) in a text editor. You'll see conflict markers that look like this:

```plaintext
<<<<<<< HEAD
Your changes
=======
Incoming changes
>>>>>>> <commit-hash>
```

- The `<<<<<<< HEAD` marker indicates the start of your changes.
- The `=======` marker separates your changes from the incoming changes.
- The `>>>>>>> <commit-hash>` marker indicates the end of the incoming changes.

Your task is to manually resolve these conflicts by editing the file to keep the changes you want. Remove the conflict markers and the unwanted changes, leaving only the desired code. Save the file.

## Step 3: Add and Commit the Resolved Changes

After manually resolving the conflicts, you need to stage the modified file and commit it. Use the following commands:

```bash
git add <conflicted-file>
git commit
```

In the commit message, it's a good practice to mention that you resolved conflicts and provide a brief description of the changes you made.

## Step 4: Push the Changes

Once you've resolved conflicts and committed the changes, you can push them back to the remote repository:

```bash
git push origin <branch-name>
```

## Step 5: Verify and Test

After pushing the changes, it's essential to ensure that the code still works as expected. Test your changes thoroughly to make sure that the conflict resolution did not introduce any new issues.

## Step 6: Communicate with Team

Communication is vital in collaborative development. If you encounter conflicts while pulling changes from the remote repository, inform your team about the conflicts and how you resolved them. This helps keep everyone on the same page.

That's the basic process for managing Git conflicts. It's essential to maintain good communication with your team and follow best practices for conflict resolution to ensure a smooth collaborative development process.

----------------------
## Git Rebase
----------------------

In Git, `git rebase` is a command used to reorganize the commit history of a branch. It allows you to integrate changes from one branch into another, often resulting in a more linear and organized commit history.

### Step 1: Checkout the Target Branch

First, make sure you are on the branch where you want to apply changes. For example, to update the `master` branch with changes from another branch, use:

```bash
git checkout master
```

### Step 2: Start the Rebase

Initiate the rebase process by running the following command, replacing `<source-branch>` with the name of the branch containing the changes you want to apply:

```bash
git rebase <source-branch>
```

### Step 3: Resolve Conflicts

If there are conflicts between the changes in the source branch and the target branch, Git will pause the rebase process. You will need to manually resolve the conflicts in the affected files.

Use your text editor to edit the conflicted files, remove conflict markers, and retain the desired changes. After resolving conflicts, use:

```bash
git add <conflicted-file>
```

to stage the resolved changes, and then continue the rebase:

```bash
git rebase --continue
```

### Step 4: Complete the Rebase

Once the rebase is successful, Git will have applied the changes from the source branch onto the target branch. You can now use:

```bash
git log
```

to view the updated commit history. It should appear more linear and organized.

---------------------
# Git Stash
---------------------

In Git, the `git stash` command is a powerful tool for temporarily saving changes in your working directory without committing them. This is particularly useful when you need to switch branches, pull changes, or perform other Git operations without losing your current work. The changes are stored in a "stash" and can be reapplied later.

## Stash Changes

To stash your changes, use the following command:

```bash
git stash
```

This command takes all the changes in your working directory and stores them in a new stash.

## List Stashes

You can list your stashes to see what's stored. Use the following command:

```bash
git stash list
```

This command will display a list of stashes, each with a unique name, typically in the format `stash@{n}`.

## Apply Stash

To apply the most recent stash, use the following command:

```bash
git stash apply
```

If you want to apply a specific stash, specify its name, e.g., `stash@{n}`:

```bash
git stash apply stash@{2}
```

This will apply the changes from the specified stash to your working directory.

## Pop Stash

If you want to apply the most recent stash and remove it from the stash list, use the `git stash pop` command:

```bash
git stash pop
```

## Drop Stash

To remove a stash without applying its changes, you can use the `git stash drop` command:

```bash
git stash drop stash@{2}
```

## Clear All Stashes

If you want to remove all stashes, you can use the `git stash clear` command:

```bash
git stash clear
```

`git stash` is a powerful and versatile tool for temporarily saving your work when working with Git. It allows you to switch branches, perform various Git operations, and then reapply your changes when you're ready. Make sure to use it wisely to prevent data loss and effectively manage your work.






