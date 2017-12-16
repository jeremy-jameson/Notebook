# Git

Saturday, December 16, 2017
5:22 AM

## # Configure basic Git options

```PowerShell
git config --global user.email "jjameson@technologytoolbox.com"
git config --global user.name "Jeremy Jameson"

git config --global color.ui true
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
```
