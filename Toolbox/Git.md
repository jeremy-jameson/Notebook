# Git

Saturday, December 16, 2017
5:22 AM

## # Configure basic Git options

```PowerShell
git config --global user.email "jjameson@technologytoolbox.com"
git config --global user.name "Jeremy Jameson"

git config --global color.ui true
```

## # Configure Git to use Visual Studio Code as core editor

```PowerShell
git config --global core.editor "'C:\Program Files\Microsoft VS Code\Code.exe' --wait"
```

## # Configure Git to use SourceGear DiffMerge

```PowerShell
git config --global diff.tool diffmerge

git config --global difftool.diffmerge.cmd  '"C:/NotBackedUp/Public/Toolbox/DiffMerge/x64/sgdm.exe \"$LOCAL\" \"$REMOTE\"'
```

### Reference

**Git for Windows (MSysGit) or Git Cmd**\
From <[https://sourcegear.com/diffmerge/webhelp/sec__git__windows__msysgit.html](https://sourcegear.com/diffmerge/webhelp/sec__git__windows__msysgit.html)>

```PowerShell
cls
```

## # Install Git PowerShell module

```PowerShell
Set-PSRepository -Name PSGallery -InstallationPolicy Trusted

Install-Module -Name 'posh-git'
```

```PowerShell
cls
```

## # Clone repository

```PowerShell
mkdir src\Repos

cd src\Repos

git clone https://techtoolbox.visualstudio.com/DefaultCollection/_git/Training


# Show differences

git diff

# Unstage files

git status

git reset HEAD <file>

# Discard changes

git checkout -- <file>

# Undo a commit

git reset --soft HEAD^

(move to commit before HEAD)

# Add to a commit

git add <file>

git commit --amend -m <message>

# Undo last commit and all changes

git reset --hard HEAD^

# Undo last two commits and all changes

git reset --hard HEAD^^

# Adding a remote

git remote add <name> <address>

git remote add origin https://techtoolbox.visualstudio.com/DefaultCollection/_git/Training

Note that "origin" is the name for the remote (by convention "origin" is typically used)

# View remotes

git remote -v

# Push to remote

git push -u origin master

Password caching - https://help.github.com/articles/set-up-git

# Pull from remote

git pull

# Create branch

git branch <name>

# Switch to branch

git checkout <branch>

# Merge branch

git checkout master

git merge <branch>

# Create and switch to branch (single step)

git checkout -b <branch>

# Create a remote branch

git checkout -b <name>

git push origin <branch name>

# List remote branches

git branch -r

# Pull remote branch

git checkout <branch name>

# Show remote branches with tracking info

git remote show origin

# Remove a branch

# Remove remote branch

git push origin :<branch name>

This only deletes the remote branch (not the local branch)

# Delete local branch

git branch -d <branch name>

If there are commits that have not been merged, an error occurs. If you are sure you want to delete it:

git branch -D <branch name>

# Clean up local branches when remote branches have been deleted

git remote show origin

git remote prune origin

# Tagging

# List all tags

git tag

# Add a tag

git tag -a <tag name> -m <message>

# Push new tags

git push --tags

# Show differences

git diff HEAD          # latest commit

git diff HEAD^         # parent of latest commit

git diff HEAD^^        # grandparent of latest commit

git diff HEAD~5        # 5 commits ago

git diff HEAD^..HEAD   # second most recent commit vs. most recent

# Show history of each line in a file

git blame <file> --date short

# Exclude files and folder

Notepad .gitignore

<file>
<folder>/

# Untrack files

git rm --cached <file>
```
