---
layout: post
title: Git Cheat Sheet
category: tips
tags: tips git cheatsheet
keywords: git
description:
---

### Configure tooling

Configure user information for all local repositories

```bash
$ git config --global user.name "[name]"           # Sets the name you want atached to your commit transactions
$ git config --global user.email "[email address]" # Sets the email you want atached to your commit transactions
$ git config --global color.ui auto                # Enables helpful colorization of command line output
```

### Create repositories

Start a new repository or obtain one from an existing URL

```bash
$ git init [project-name] # Creates a new local repository with the specified name
$ git clone [url]         # Downloads a project and its entire version history
```

### Make changes

Review edits and craf a commit transaction

```bash
$ git status                            # Lists all new or modified files to be commited
$ git add [file]                        # Snapshots the file in preparation for versioning
$ git reset [file]                      # Unstages the file, but preserve its contents
$ git diff                              # Shows file differences not yet staged
$ git diff --staged                     # Shows file differences between staging and the last file version
$ git commit -m "[descriptive message]" # Records file snapshots permanently in version history
```

### Group changes

Name a series of commits and combine completed efforts

```bash
$ git branch                  # Lists all local branches in the current repository
$ git branch [branch-name]    # Creates a new branch
$ git checkout [branch-name]  # Switches to the specified branch and updates the working directory
$ git merge [branch]          # Combines the specified branch’s history into the current branch
$ git branch -d [branch-name] # Deletes the specified branch
```

### Refactor filenames

Relocate and remove versioned files

```bash
$ git rm [file]                         # Deletes the file from the working directory and stages the deletion
$ git rm --cached [file]                # Removes the file from version control but preserves the file locally
$ git mv [file-original] [file-renamed] # Changes the file name and prepares it for commit
```

### Suppress tracking

Exclude temporary files and paths

```bash
 *.log
 build/
 temp-*
# A text file named .gitignore suppresses accidental versioning of files and paths matching the specified patterns
$ git ls-files --other --ignored --exclude-standard
# Lists all ignored files in this project
```

### Save fragments

Shelve and restore incomplete changes

```bash
$ git stash      # Temporarily stores all modified tracked files
$ git stash pop  # Restores the most recently stashed files
$ git stash list # Lists all stashed changesets
$ git stash drop # Discards the most recently stashed changeset
```

### Review history

Browse and inspect the evolution of project files

```bash
$ git log                                   # Lists version history for the current branch
$ git log --follow [file]                   # Lists version history for a file, including renames
$ git diff [first-branch]...[second-branch] # Shows content differences between two branches
$ git show [commit]                         # Outputs metadata and content changes of the specified commit
```

### Redo commits

Erase mistakes and craft replacement history

```bash
$ git reset [commit]        # Undoes all commits after [commit], preserving changes locally
$ git reset --hard [commit] # Discards all history and changes back to the specified commit
```

### Synchronize changes

Register a repository bookmark and exchange version history

```bash
$ git fetch [bookmark]          # Downloads all history from the repository bookmark
$ git merge [bookmark]/[branch] # Combines bookmark’s branch into current local branch
$ git push [alias] [branch]     # Uploads all local branch commits to GitHub
$ git pull                      # Downloads bookmark history and incorporates changes
```

### Useful snippet

1. recover all deleted files [link](http://stackoverflow.com/questions/953481/find-and-restore-a-deleted-file-in-a-git-repository)

```bash
$ git ls-files -d | xargs git checkout --
```

### Reference
1. [Try Git](https://try.github.io/levels/1/challenges/1)
2. [github-git-cheat-sheet](https://training.github.com/kit/downloads/github-git-cheat-sheet.pdf)
