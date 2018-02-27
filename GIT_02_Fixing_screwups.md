# Part 2: Fixing your screw-ups

Optional useful commands  
`$ git log`: shows all latest logs  
`$ git reflog`  

`ESC` + `:wq` : exiting the merge or commit message window


<br>

### Undo the changes made on an R file (not staged yet!) and return the previous version
$ git checkout filename.R

### Undo a commit without losing the actual commit
$ git revert hashcode_to_revertcommit


### Removing files from the staging area to working directory
$ git reset



### Different resets

`$ git reset –soft paste_haschode_here`: Reset to specified commit, but will keep changes in the staging area  
`$ git reset paste_haschode_here`: Default setting. Reset to specified commit, changes are in the working directory rather than the staging area  
`$ git reset –hard paste_haschode_here`: Really don’t want the changes made, I want to go back to where we were before. All files will match the state they were at the previous commit. Clears staging area and working directory on tracked files.  

<br>

## I commited on master instead of my branch 

```
#find the previous hashcode
$ git log 

#checkout the branch it should be commited on
$ git checkout BranchName

#cherry-pick commit from master to branch
$ git cherry-pick paste_haschode_here

#shows log on branchname with the new commit
$ git log

#deleting the commit on the master branch
$ git checkout master
$ git reset 
```


<br>

## Miscellaneous:

Resetting to 300 minutes ago: 
`$ git reset --hard master@{"300 minutes ago"}` [(Source)](https://twitter.com/data_stephanie/status/968226587547258886)
