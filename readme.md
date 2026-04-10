
# Git & GitHub 

## Introduction

**Git** is a free and open-source version control system used to track changes in files and source code. It allows developers to manage project history, collaborate efficiently, and safely revert to previous versions when issues occur. Git is available for **Windows**, **macOS**, and **Linux**, and runs locally on your computer.

Think of Git like a **checkpoint system** in a game:  
you create save points (commits), and if something breaks, you can go back to a previous stable checkpoint.

**GitHub** is a **hosting service** for Git repositories.  
Git is the software, and GitHub is the online platform that hosts Git repositories and enables collaboration over the internet.

---

## VCS (Version Control System)

A **Version Control System (VCS)** helps track file changes over time, manage multiple versions of a project, and enable collaboration among developers.

---

## Basic Git Commands

### Check Git Version
```bash
git --version
````

Displays the installed Git version.

---

### Present Working Directory

```bash
pwd
```

Shows the current directory path in the terminal.

---

### Repository Status

```bash
git status
```

Displays the current state of the repository (staged, unstaged, untracked files).

**Common error:**

```
fatal: not a git repository (or any of the parent directories): .git
```

This means the current folder is **not initialized as a Git repository**.

---

## Git Configuration

Set your global Git identity:

```bash
git config --global user.email "your-email@example.com"
git config --global user.name "Your Name"
```

---

## Initialize a Repository

```bash
git init
```

Initializes Git tracking in the current directory and creates a hidden `.git` folder.
This is the starting point of version control for a project.

---

## Git Workflow

**Flow:**

```
Write → Add → Commit → Push
```

### Steps:

1. **Initialize repository**

   ```bash
   git init
   ```

2. **Stage changes**

   ```bash
   git add
   ```

3. **Commit changes (local only)**

   ```bash
   git commit
   ```

4. **Push to GitHub (remote)**

   ```bash
   git push
   ```

> `add` and `commit` save data **locally**
> `push` sends data to **GitHub (remote repository)**

---

## Staging Files

### Add specific files

```bash
git add <FILE_NAME_1> <FILE_NAME_2>
```

### Add all files

```bash
git add .
```

---

## Commit Changes

```bash
git commit -m "Commit message"
```

* `-m` → commit message flag
* Creates a checkpoint in project history

---

## View Commit History

### Full log

```bash
git log
```

Shows:

* Commit ID (hash)
* Branch
* Author
* Date
* Commit message

---

### One-line log

```bash
git log --oneline
```

Compact commit history (one line per commit).

---

## Git Editor (VIM & VS Code)

### Default Editor: VIM

If you commit **without `-m`**, Git opens **VIM** to enter a commit message.

---

### Set VS Code as Default Git Editor

#### Step 1: Install VS Code command

* Press: `Command + Shift + P`
* Search: `Shell Command: Install 'code' command in PATH`

#### Step 2: Configure Git

```bash
git config --global core.editor "code --wait"
```

Now, when committing without `-m`, VS Code opens `COMMIT_EDITMSG`.
After writing the message and closing the file, Git automatically completes the commit.

---

## `.gitignore` File

`.gitignore` is used to exclude files/folders from tracking.

Example:

```
node_modules
.env
dist
```

### Notes:

* Git does **not track empty folders**
* To keep empty folders, add a file:

```
.gitkeep
```

---

## Branches in Git

Branches allow parallel development on different features or fixes.

### HEAD

`HEAD` is a pointer to the current branch and latest commit you are working on.

> Default branch used to be `master`
> Now conventionally named `main`

---

## Branch Commands

### List branches

```bash
git branch
```

### Create branch

```bash
git branch bug-fix
```

### Switch branch

```bash
git switch bug-fix
```

### Create + switch branch

```bash
git switch -c dark-mode
```

### Checkout (older command)

```bash
git checkout orange-mode
```

---

## Merging Branches

Merging brings changes from one branch into another.

### Types of merges:

### 1. Fast-Forward Merge

Occurs when branches **have not diverged**.
No conflicts, simple history move forward.

```bash
git checkout main
git merge bug-fix
```

---

### 2. 3-Way Merge

Occurs when branches **have diverged**.
Git uses:

* Common ancestor
* Tip of branch A
* Tip of branch B

Creates a **merge commit**.

```bash
git checkout main
git merge bug-fix
```

### Difference from fast-forward:

* Fast-forward → no conflicts
* 3-way merge → possible conflicts (manual resolution required)

---

## Merge Conflicts

Example conflict markers:

```text
<<<<<<< HEAD
current branch code
=======
code from another branch
>>>>>>> bug-fix
```

You must manually resolve conflicts and choose what to keep.
VS Code provides a built-in merge conflict resolution tool.

---

## Branch Management

### Rename a branch

```bash
git branch -m <old-branch-name> <new-branch-name>
```

### Delete a branch

```bash
git branch -d <branch-name>
```

### Checkout a branch

```bash
git checkout <branch-name>
```

### List all branches

```bash
git branch
```

---

## Summary

Git provides:

* Version tracking
* History management
* Collaboration
* Safe rollback
* Parallel development
* Branching and merging workflows

GitHub provides:

* Online hosting
* Collaboration tools
* CI/CD integration
* Pull requests
* Project visibility

---

## Developer Notes

* Git = Software
* GitHub = Hosting service
* Commits = Checkpoints
* Branches = Parallel development
* Merge = Combine work
* Conflicts = Manual resolution

---
# Diff, Stash, Tags, Rebase, and Recovery in Git

This section covers advanced Git concepts that help you inspect changes, manage temporary work, mark releases, rewrite history, and recover lost commits.

---

## Git Diff

`git diff` is an informative command used to view differences between files, commits, or branches. It helps you review changes before committing them.

### View Staged Changes
When files are in the staging area and you want to review them before committing:
git diff --staged

---

## How to Read Diff Output

Key symbols and markers in `git diff` output:

* `a/` → original file
* `b/` → updated file
* `---` → original file version
* `+++` → updated file version
* `@@` → line numbers and position of changes
* `-` → removed line
* `+` → added line

### Example

```diff
diff --git a/01_file.text b/01_file.text
index 2c89356..2609bf1 100644
--- a/01_file.text
+++ b/01_file.text
@@ -1,3 +1,5 @@
 This Is First File

-we check for that
\ No newline at end of file
+we check for that
+
+this is a change to verify how the "git diff --staged" command works
\ No newline at end of file

```

## Comparing Differences

### Compare Two Branches

```bash
git diff <branch-1> <branch-2>
```

or

```bash
git diff branch-1..branch-2
```

### Compare Two Commits

```bash
git diff <commit-hash-1> <commit-hash-2>
```

---

## Git Stash

`git stash` temporarily saves uncommitted changes so you can switch branches without committing or losing work.

This is useful when Git prevents you from switching branches due to local changes.

### Example Scenario

You have uncommitted changes on `main` and want to switch to `bug-fix`:

```bash
git checkout bug-fix
```

Error:

```
error: Your local changes to the following files would be overwritten by checkout:
  02_file.text
Please commit your changes or stash them before you switch branches.
```

### Save Changes Temporarily

```bash
git stash
```

With a message:

```bash
git stash push -m "Save this ABC"
```

---

## View Stash List

```bash
git stash list
```

---

## Apply Stash

### Apply Most Recent Stash

```bash
git stash apply
```

### Apply a Specific Stash

```bash
git stash apply stash@{0}
```

---

## Apply and Remove Stash

```bash
git stash pop
```

---

## Drop a Stash

```bash
git stash drop
```

---

## Apply Stash to a Specific Branch

```bash
git stash apply stash@{0} <branch-name>
```

---

## Clear All Stashes

```bash
git stash clear
```

---

## Git Tags

Git tags are like **sticky notes** used to mark specific commits, typically for **releases or versions** (e.g., `v1.0.0`).

They help identify important points in project history such as production releases.

---

## Git Rebase

Rebase is used to move or replay commits from one branch onto another by changing the base commit.

> ⚠️ Rebase is usually performed on **feature branches**, not on the `main` branch.

### Rebase a Branch onto Main

```bash
git rebase main
```

---

### Resolving Rebase Conflicts

If conflicts occur:

1. Resolve conflicts manually (VS Code merge tools can help)
2. Stage the resolved files:

```bash
git add <resolved-files>
```

3. Continue rebase:

```bash
git rebase --continue
```

---

## Git Reflog

`git reflog` records every change made to `HEAD`, including commits, resets, rebases, and checkouts.

```bash
git reflog
```

It provides **more detailed history** than `git log`.

---

## Find a Specific Commit

```bash
git reflog <commit-hash>
```

---

## Recover Lost Commits or Changes

Sometimes you may want to go back to a previous commit and discard newer changes.

### Reset to a Specific Commit

```bash
git reset --hard <commit-hash>
```

### Reset Using HEAD Reference

```bash
git reset --hard HEAD@{1}
```

* `HEAD@{1}` means “go back one step in reflog history”
* Useful for undoing recent actions

> ⚠️ `--hard` will permanently delete uncommitted changes

---

## Git SSH Configuration

To securely connect GitHub using SSH, follow the official GitHub documentation:

👉 [https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)

---

## GitHub Repository Setup (Local to Remote)

### Steps

1. Create a repository on GitHub
2. In your local project directory, run:

```bash
git init
git add .
git commit -m "COMMIT-MESSAGE"
git branch -M main
git remote -v
git remote add origin <REPO_URL>
git push -u origin main
```

### Explanation

* `git init` → initialize Git repository
* `git add .` → stage all files
* `git commit` → create first commit
* `git branch -M main` → rename branch to `main`
* `git remote -v` → verify remote URLs
* `origin` → alias for repository URL
* `-u` → set upstream branch (future `git push` works without arguments)

---

## Summary

* `git diff` → inspect changes
* `git stash` → temporarily save work
* `git tag` → mark releases
* `git rebase` → rewrite commit history
* `git reflog` → recover lost commits
* `git reset --hard` → rollback safely (with caution)

These tools make Git powerful, flexible, and safe for professional development workflows.
