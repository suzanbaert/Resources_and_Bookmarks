 # GIT Basics

**Contents:**

+ [GIT Vocabulary](#git-vocabulary)
+ [Initializing your repository set up](#initializing-your-repository-set-up)
+ [Basic GIT workflow](#basic-git-workflow)
+ [Working with branches](#working-with-branches)
+ [GIT Tags](#git-tags)
+ [GIT Stash](#git-stash)
+ [Renaming, moving and deleting files/folders/repos](#renaming-moving-and-deleting)



<br>

## GIT Vocabulary

+ `origin`: default name of the remote repository

+ `master`: name of your default branch, but also a ref for the last commit on master.

+ `master^1` is a synonym for `master^` and `master~1` and refers to the parent commit of master. In other words: this is the last but one commit. You can keep counting back this way.

+ `HEAD`: reference to the last commit of the branch you are currently on. If you are on the master branch, then HEAD and master will refer to the same commit.

+ `HEAD^1` is the same as `HEAD^` and `HEAD~1` and is the parent commit of HEAD, i.e. the last but one commit on this branch.

<br><hr>
## Initializing your repository set up

### Starting a completely new project
Step 1: Make a new repository on github and grab the link from the clone with SSH option.

Step 2: Make a new project in R studio with Git control and copy the link.

### An existing project on your PC that is not yet tracked by Git
First make a remote repository on Github and grab the link from the clone with SSH option.

Browse through the right folder in the command line, or browse in your normal explorer to the right folder and right click “Git Bash here”:

```
#-- Initialize git tracking
$ git init

-- Add the remote repository
$ git remote add origin YourGithubLink

-- Add and commit the files
$ git add -A
$ git commit m "commit message"

-- Make the link between the local and remote repository
$ git push -u origin master
```


### An existing project on Github but not yet on your pc
Browse to the right root folder and type in Bash:
```
-- Making a copy of the repository
$ git clone TheGithubLink
```

<br><hr>

## Basic GIT workflow

Basic workflow:
+ `git add` to add from your working directory to your staging area
+ `git commit` to add the files to your local GIT
+ `git push` to add the files to your remote GIT  


```
-- Add all the files to the staging area
$ git add -A

-- Make a commit on your work
$ git commit -m "enter your commit message"

-- Push your latest commit to github
$ git push
```

Optional elements:
+ `git status`: shows which files have changed/new.  
+ `git diff`: shows the changes you made to the file  

Different ways to **add** elements to the staging area:
+ `git add filename.ext` will only stage the specific file  
+ `git add *.R` will stage all .R files  
+ `git add data/` will add the data folder and its contents
+ `git add .` stages all new and modified files, but does not remove any deleted files  
+ `git add -u` stages all modified and deleted files, but does not stage any new (and therefore untracked) files  
+ `git add -A` stages all new, modified and deleted

**Cleaning up your working directory:**
+ `git clean -n`: dry run, shows all files that will be deleted
+ `git clean -df`: removes all untracked files/folders


Note on adding files to the remote:  
+ When it is your first push from a repo, you will first have to make the link between the local and remote repository via: `git push  --set-upstream origin master`, or shorter `git push -u origin master`. As of then, `git push` will refer to the upstream branch you've set: i.e. origin/master.


**Gitignore:**  
Add a file `.gitignore` which lists all files that should be ignored by GIT. To add files to `.gitignore` within the command line:

+ `touch .gitignore`: will create a .gitignore file
+ `echo *.Rproj > .gitignore`: create and add R project file to ignore


<br>

**Notes for collaborative repos:**

If a collaborative repo, the origin/master might be ahead of yours due to work from other people, and you won't be able to just push content.  

Options:
+ `git pull`: with fetch the latest from the remote and merge them into you changes. this might result in a merge conflict that has to be resolved.

+ `git fetch`: will only fetch the latest changes but not merge them. it will tell you which branches have changed. by running `git diff master origin/master` you can find out what the changes are, and merge manually.

+ `git pull --rebase`: will pull commits from remote, and rebase your current commits on top of the upstream changes. it will commit your changes on top of what others have done.

You can use git diff to check the difference between the remote and the local repo: `git diff master origin/master` will check the differences between the latest commit locally (master) and latest commit remotely (origin/master)



<br><hr>

## Working with branches

Bigger changes are best made on a branch first, and only merged onto the master after the work is completed.
You can create the remote branch at any time, after checkout or only just before the first push. I find it easiest to make it straight after creation so I don’t forget
Adding -u ensures that the local and remote branch are connected.  


Optional useful commands:
+ `git branch -a`: lists all existing branches (local and remote)
+ `git branch --merged`: lists all merged branches

Step 1: Create the new branch and check it out so you start working on that branch:

```
-- Creating a new branch locally:
$ git branch YourBranchName

-- Creating the new branch remotely:
$ git push -u origin YourBranchName

-- Swapping to the new branch to work on:
$ git checkout YourBranchName
```
Step 2: Work on the branch as you are used to: add, commit, push to the new branch (link is already cemented in previous `git push -u` so no need to keep repeating it).
```
git add -A
git commit -m "added something to new branch"
git push origin YourBranchName
```

Steps 1 & 2 in one go:
```
-- checkout a newly made branch:
$ git checkout -b NewBranch

-- make your changes and create remote branch when pushing the first time:
$ git add -A
$ git commit -m "message"
$ git push -u origin Newbranch
```

Step 3: If you are ready to merge:
```
-- Changing back to master branch:
$ git checkout master

-- Make sure your copy of master is up to date:
$ git pull origin master

-- Merging the branch on the master:
$ git merge YourBranchName

-- Push the changes to master to the remote repository:
$ git push origin master

-- Deleting the branch remotely:
$ git push --delete origin YourBranchName

-- Deleting the branch locally:
$ git branch -d YourBranchName
```

When there are no changes to the master, the branch can be merged "fast forward". Sometimes it's good to force a merge commit, so the information is in the history. To do so: `$ git merge --no-ff BranchName`.  

Instead of merging, you can also `git rebase` (see later).  


<br>
<hr>

## GIT tags

To tag the current state of master (tag will be added to the last commit):
```
$ git tag v0.1
```


Useful:
+ `git tag`: To list all tags
+ `git tag --force TagName`: Updates a previous tag to the latest commit
+ `git tag --delete TagName`: Deletes a tag


<br><hr>

## GIT Stash

To stash some changes to perhaps use later again. By making a stash, you bring the repo back to the previous committed stage.

Making a new stash
```
-- to stash tracked files:
$ git stash save

-- to also stash untracked files:
$ git stash save -u
```

Restoring the stashed changes: default the latst stash
```
$ git stash pop
```


Useful:
+ `git stash list`: lists stashes
+ `git stash drop`: discards the most recent stash
+ `git stash clear`: clearing all previous stashes



<br>
<hr>



## Renaming, moving and deleting

### Files and folders

If you've renamed or moved a file, GIT by default will see it as a deletion and a new addition of a file, and not realize that these files are related.  
You can also get GIT to rename/move a file via `git mv` in which case history will be preserved. After renaming/moving, `git status` will not show an addition/deletion but a rename that is already staged and ready to be committed.

To rename a file:
```
$ git mv oldname.ext newname.ext
```

To move a file to a folder:
```
$ git mv file.ext foldername/file.ext
```

If the new location already has a file with the same filename that you want to overwrite, use `--force` or shorter `-f`
```
$ git mv --force file.ext foldername/file.ext
```

To delete a file:
```
$ git rm file.ext
```

To delete a folder with files:
```
-- dry run first to see what check what is inside:
$ git rm -r --dry-run folder/

--delete folder and files inside:
$ git rm -r folder/
```

### Renaming a remote repository

If you've changed your repository name on Github, you'll have to you change your local repository's remote links. You can delete and re-clone of course, but you can also just change your remote link.  

To view your current remote links:   
`$ git remote -v`  

To change your remote URL in one go:  
`$ git remote set-url origin YourNewLink`  

Alternative option in multiple steps:  
Delete current remote: `$ git remote rm origin`  
Add new remote: `$ git remote add origin YourNewLink`    
Confirm the link: `$ git push -u origin master`  


<br><hr>

## Resources

+ [Atlassian GIT tutorials](https://www.atlassian.com/git/tutorials/git-stash)

+ [Git in practice, by Mike McQuaid](https://github.com/GitInPractice/GitInPractice#readme)

+ [Youtube tutorials by Corey Schafer](https://www.youtube.com/watch?v=HVsySz-h9r4&list=PL-osiE80TeTuRUfjRe54Eea17-YfnOOAx)
