# GIT Basics

## Initializing your repository set up

### Starting a completely new project
Step 1: Make a new repository on github and grab the link from the clone with SSH option.
Step 2: Make a new project in R studio with Git control and copy the link. 

### An existing project on your PC that is not yet tracked by Git
First make a remote repository on Github and grab the link from the clone with SSH option.
Browse through the right folder in the command line, or browse in your normal explorer to the right folder and right click “Git Bash here”:

```
#Initialize git tracking
$ git init

#Add the remote repository
$ git remote add origin YourGithubLink

#Add and commit the files
$ git add -A
$ git commit m "commit message"

#Make the link between the local and remote repository
$ git push -u origin master
```


### An existing project on Github but not yet on your pc
Browse to the right root folder and type in Bash:
```
#Making a copy of the repository 
$ git clone TheGithubLink
```

<br>

## Basic GIT workflow

Basic workflow: git add, git commit, git push.

Optional elements
`git status`: shows which files have changed/new..  
`git diff`: shows the changes you made to the file  

```
#First check whether you have the latest material (if you are collaborating or work on multiple pc's on the same repo)
$ git pull

#Add all the files to the staging area
$ git add -A

#Make a commit on your work
$ git commit -m "enter your commit message"

#Push your latest commit to github
$ git push origin master
```

The above code adds every change to the staging area. You can specify filenames, filetypes, folders etc...  
+ `git add filename.ext` will only stage the specific file  
+ `git add *.R` will stage all .R files  
+ `git add data/` will add the data folder  
+ `git add .` stages all new and modified files, but does remove any deleted files  
+ `git add -u` stages all modified and deleted files, but does not stage any new (and therefore untracked) files  
+ `git add -A` stages all new, modified and deleted  


If you want to remove untracked files/folders: `git clean -df`

<br>

## Working with branches

Bigger changes are best made on a branch first, and only merged onto the master after the work is completed.
You can create the remote branch at any time, after checkout or only just before the first push. I find it easiest to make it straight after creation so I don’t forget
Adding -u ensures that the local and remote branch are connected.  


Optional useful commands:
`git branch -a`: lists all existing branches (local and remote)
`git branch --merged`: lists all merged branches

**First create the new branch**
Create the new branch and check it out so you start working on that branch:

```
#Creating a new branch locally
$ git branch YourBranchName

#Creating the new branch remotely
$ git push -u origin YourBranchName

#Swapping to the new branch to work on
$ git checkout YourBranchName
```

Work on the branch as you are used to: add, commit, push to the branch (not to master obviously!)
```
git add -A
git commit -m "added something to new branch"
git push origin YourBranchName
```

If you are ready to merge:
```
#Changing back to master branch
$ git checkout master

#Make sure your copy of master is up to date
$ git pull origin master

#Merging the branch on the master
$ git merge YourBranchName

#Optional: check the merged branches, YourBranchName should be in the list
$ git branch --merged

#Push the changes of master to the remote repository
$ git push origin master

#Deleting the branch locally
$ git branch -d YourBranchName

#Deleting the branch remotely
$ git push origin --delete YourBranchName
