-------------------------------
# Git Basics Commands
-------------------------------
![image](https://github.com/asiandevs/DevOps-Git/assets/37457408/2cce3b6f-fd82-4566-afff-7af34832063c)


## Local Repository

A **local repository** is the Git repository that resides on your local machine. It contains all the files, commit history, and branches related to a specific project. Your local repository allows you to make changes, create branches, commit changes, and perform other version control tasks.

Here's how to work with a local repository:

1. **Initialize a Local Repository:**
   You can create a new Git repository in a directory by using the following command:
   ```bash
   git init
   ```
** Figure Git repository structure on a local machine ** 
![image](https://github.com/asiandevs/DevOps-Git/assets/37457408/f9dd06d2-c6f1-45db-8f4c-0157f05ada89)

1. **Add Files:**
   You can add files to your local repository by using the `git add` command, which stages changes for the next commit.
   ```bash
   git add filename
   ```

2. **Commit Changes:**
   Commit your changes to the local repository with a descriptive message using the `git commit` command.
   ```bash
   git commit -m "Your commit message"
   ```
# Setup
```
git --help
```

## Create an Empty Directory
Create an empty directory named "hello", then create a file named hello.rb with the contents below.

```bash
mkdir gitdemo
cd gitdemo
```
** gitdemo - I am making this as a local repository **

## Create the Repository
You now have a directory with a single file. To create a Git repository from that directory, run the `git init` command.

```bash
git init
```

## Add the Program to the Repository
Now let's add the file to the repository.

```bash
touch gitdemo01.txt
git add .
git commit -m "First Commit"
```

## Check the Status of the Repository
Use the `git status` command to check the current status of the repository.

```bash
git status
```
![image](https://github.com/asiandevs/DevOps-Git/assets/37457408/2539d2a0-1c59-4c81-9f45-4396fa9de5c4)

The status command reports that there is nothing to commit. This means that the repository has all the current state of the working directory. There are no outstanding changes to record.

We will use the `git status` command to continue monitoring the state between the repository and the working directory.

## Staging and Committing

```
git add .
git commit -m "First Commit"
```
## View the History of the Project
Getting a listing of what changes have been made is the function of the `git log` command.

```bash
git log
```

Summary Log
```
# git log --oneline
7c888be (HEAD -> master) First Commit
```

## Git Internals: The .git Directory
### Goals
Learn about the structure of the .git directory

### The .git Directory
Time to do some exploring. First, from the root of your project directory…

```bash
ls -C .git
```

You should see a bunch of directories with 2-letter names. The directory names are the first two letters of the SHA-1 hash of the object stored in Git.

### Deeper into the Object Store
```bash
ls -C .git/objects/<dir>
```

Look in one of the two-letter directories. You should see some files with 38-character names. These are the files that contain the objects stored in Git. These files are compressed and encoded, so looking at their contents directly won't be very helpful, but we will take a closer look in a bit.

## Config File
```bash
cat .git/config
```

This is a project-specific configuration file. Config entries in here will override the config entries in the .gitconfig file in your home directory, at least for this project.

### Aliases
`git status`, `git add`, `git commit`, and `git checkout` are such common commands that it is useful to have abbreviations for them.

Add the following to the .gitconfig file in your $HOME directory.

**.gitconfig**
```plaintext
[alias]
  co = checkout
  ci = commit
  st = status
  br = branch
  hist = log --pretty=format:'%h %ad | %s%d [%an]' --graph --date=short
  type = cat-file -t
  dump = cat-file -p
```

## to ignore files -- place the file name in .gitignore file 
```
vi .gitignore
git add .gitignore
git commit -m "adding gnore files"
```
- Check from OS level and compare from the git level
```
ls -al
```
```
git ls-files
```
### See the changes in file 
```
touch file3.txt
git diff file3.txt
git diff --staged file3.txt
git add file3.txt
git diff --staged file3.txt
```
### remove 
- Both from Working Directory and Local Repository
```
git rm file5.txt
```
- FRom Local Repository but not from Working Directory
```
git rm --cached file4.txt
git status
git commit -m "teste"
git restore --staged file4
```

# Git Reset Modes

In Git, the `git reset` command is used to adjust the current branch and move the HEAD to a specific commit. Git reset comes with three primary modes: soft, mixed, and hard, each with distinct behaviors regarding the staging area and working directory.
** Note: First get the commitid

![image](https://github.com/asiandevs/DevOps-Git/assets/37457408/cad8d30b-ec94-45d1-85a3-258e9c078070)

Soft Reset: Use when you want to recommit changes with modifications or when you mistakenly committed changes to the wrong branch.

Mixed Reset: Use when you want to unstage changes but keep them in your working directory. It's helpful for reorganizing commits before pushing.

Hard Reset: Use with caution, as it permanently removes changes. It's suitable when you want to completely discard changes or return to a previous state.

```
git log --oneline
```
## Soft Reset

- Command: `git reset --soft <commit>`
- Behavior:
  - Moves the HEAD and branch pointer to the specified commit.
  - Resets the staging area (index) to match the state of the specified commit.
  - Retains the changes in your working directory.
- Use case:
  - Uncommit changes while preserving them for further modifications and recommit.

## Mixed Reset (Default)

- Command: `git reset --mixed <commit>` (or simply `git reset <commit>`)
- Behavior:
  - Moves the HEAD and branch pointer to the specified commit.
  - Resets the staging area (index) to match the state of the specified commit.
  - Does not preserve the changes in your working directory.
- Use case:
  - Unstage changes and start over without losing your work.

## Hard Reset

- Command: `git reset --hard <commit>`
- Behavior:
  - Moves the HEAD and branch pointer to the specified commit.
  - Resets the staging area (index) and the working directory to match the state of the specified commit.
  - Discards all changes, potentially resulting in data loss.
- Use case:
  - Be extremely cautious when using a hard reset, as it can lead to permanent data loss.

Remember to replace `<commit>` with the actual commit reference (e.g., a commit hash, branch name, or relative commit reference) you want to reset to. Always exercise caution when performing hard resets, as they can result in the loss of data that may not be easily recoverable.
```