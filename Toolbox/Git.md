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
```

> **Note**
> 
> PowerShellGet requires NuGet provider version '2.8.5.201' or newer to interact with NuGet-based repositories. If prompted to install and import the NuGet provider, type **Y** and press **Enter** to continue.

```PowerShell
Install-Module -Name 'posh-git'

PackageManagement\Install-Package : The following commands are already available on this system:'TabExpansion'. This module 'posh-git' may override the existing commands. If you still want to install this module 'posh-git', use -AllowClobber parameter.
At C:\Program Files\WindowsPowerShell\Modules\PowerShellGet\1.0.0.1\PSModule.psm1:1809 char:21
+ ...          $null = PackageManagement\Install-Package @PSBoundParameters
+                      ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : InvalidOperation: (Microsoft.Power....InstallPackage:InstallPackage) [Install-Package], Exception
    + FullyQualifiedErrorId : CommandAlreadyAvailable,Validate-ModuleCommandAlreadyAvailable,Microsoft.PowerShell.PackageManagement.Cmdlets.InstallPackage

Install-Module -Name 'posh-git' -AllowClobber
```

```PowerShell
cls
```

## # Clone repository

```PowerShell
mkdir src\Repos

cd src\Repos

git clone https://techtoolbox.visualstudio.com/DefaultCollection/_git/Training
```

### # Show differences

```PowerShell
git diff
```

### # Unstage files

```PowerShell
git status

git reset HEAD <file>
```

### # Discard changes

```PowerShell
git checkout -- <file>
```

### # Undo a commit

```PowerShell
git reset --soft HEAD^
```

(move to commit before HEAD)

### # Add to a commit

```PowerShell
git add <file>

git commit --amend -m <message>
```

### # Undo last commit and all changes

```PowerShell
git reset --hard HEAD^
```

### # Undo last two commits and all changes

```PowerShell
git reset --hard HEAD^^
```

### # Adding a remote

```PowerShell
git remote add <name> <address>

git remote add origin https://techtoolbox.visualstudio.com/DefaultCollection/_git/Training
```

Note that "origin" is the name for the remote (by convention "origin" is typically used)

### # View remotes

```PowerShell
git remote -v
```

### # Push to remote

```PowerShell
git push -u origin master
```

### # Pull from remote

```PowerShell
git pull
```

### # Create branch

```PowerShell
git branch <name>
```

### # Switch to branch

```PowerShell
git checkout <branch>
```

### # Merge branch

```PowerShell
git checkout master

git merge <branch>
```

### # Create and switch to branch (single step)

```PowerShell
git checkout -b <branch>
```

### # Create a remote branch

```PowerShell
git checkout -b <name>

git push origin <branch name>
```

### # List remote branches

```PowerShell
git branch -r
```

### # Pull remote branch

```PowerShell
git checkout <branch name>
```

### # Show remote branches with tracking info

```PowerShell
git remote show origin
```

### # Remove a branch

#### # Remove remote branch

```PowerShell
git push origin :<branch name>
```

This only deletes the remote branch (not the local branch)

#### # Delete local branch

```PowerShell
git branch -d <branch name>
```

If there are commits that have not been merged, an error occurs. If you are sure you want to delete it:

```Console
git branch -D <branch name>
```

#### # Clean up local branches when remote branches have been deleted

```PowerShell
git remote show origin

git remote prune origin
```

### # Tagging

#### # List all tags

```PowerShell
git tag
```

#### # Add a tag

```PowerShell
git tag -a <tag name> -m <message>
```

#### # Push new tags

```PowerShell
git push --tags
```

### # Show differences

```PowerShell
git diff HEAD          # latest commit

git diff HEAD^         # parent of latest commit

git diff HEAD^^        # grandparent of latest commit

git diff HEAD~5        # 5 commits ago

git diff HEAD^..HEAD   # second most recent commit vs. most recent
```

### # Show history of each line in a file

```PowerShell
git blame <file> --date short
```

### # Exclude files and folders

```PowerShell
Notepad .gitignore

<file>
<folder>/
```

### # Untrack files

```PowerShell
git rm --cached <file>
```

## Gitflow

**A successful Git branching model**\
From <[http://nvie.com/posts/a-successful-git-branching-model/](http://nvie.com/posts/a-successful-git-branching-model/)>

**Gitflow Workflow**\
From <[https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow)>

### Password caching

[https://help.github.com/articles/set-up-git](https://help.github.com/articles/set-up-git)
