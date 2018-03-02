# Part 2: Fixing your screw-ups

## Exploring history

Useful commands:
+ `$ git log`: shows all latest logs. can take additional arguments for timestamps, regex, etc...
+ `$ git log --oneline`: shows all commits on one oneline
+ `$ git log --oneline --graph`: shows a graphical representation including branching
+ `$ git reflog`: history of all times that the HEAD pointer changes, so this log includes checking out branches, and git resets. This contains only the local changes though, no what others did.

+ `$ git show hashcode`: shows the information of a certain commit
+ `$ git diff hashcode1 hashcode2`: checking differences in commits

Whenever you get into merge conflict windows, type the message and then hit `ESC` + `:wq` to exit.


<br><hr>

## Case 1: It's not staged or committed

**1. I want to get rid of changes I made in my working directory**

To get rid of changes on one file:
?? same as $ git checkout filename.R ??
```
$ git reset --hard file.ext
```

To get rid of changes in all tracked files:
```
$ git reset --hard
```

To also remove any untracked folders and files:
```
$ git clean -df
```


**2. I want to checkout another branch without committing what I have**

To force checking out a branch (all edits will be gone!):
```
$ git checkout --force OtherName
```

<br>

##  Case 2: When it's staged, but not yet committed:

**Bring the changes back to working directory**

+ `git reset HEAD file.ext`: will unstage one file

+ `$ git reset`: will unstage the files and bring them back (with the edits inside) to the working directory. This reset uses the default `git reset --mixed`.

+ `git reset --hard`: will unstage files and undo the changes made in them. Files in working directory will not contain the edits but be a copy from the previous commit.


<br>

## Case 3: When it's just been committed:

**1. Changing your commit message**

```
$ git commit --amend
```

A commit message window will show, change your commit message and press `ESC` followed by `:wq`.


**2. Add something to the latest commit**

A file that wasn't saved, or a quick typo that you've spotted. Make the change needed, and then add to the previous commit:

```
$ git add -A
$ git commit --amend
```

**3. Undoing the latest commit (but keeping changes)**

To undo a commit and bring all changes back to the working directory (to do this on just a file, add the filename).
```
$ git reset --soft HEAD^
```

**4. Undoing the latest commit (and throw away changes)**

Git revert will reset to the previous commit without erasing the commit-to-be-undone from the history.  
Find the hashcode from the latest commit:  

```
$ git revert hashcode-to-be-undone
```

Alternative is to reset a commit, which does mean it will be erased from history - usually bad for collaborative code. This will checkout the last but one commit, undoing the last one.

```
Alternative is `git reset HEAD^`
```


<br>

## Case 4: Going back to a previous commit:




<br>


## Special case 1: I committed on master instead of my branch

Note: Cherry-picking is the process of including a single commit from one branch to another. This can be done from a branch to master, or in this case from master to a branch:

```
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

## GIT rebase

!! I don't completely understand rebasing yet, so have to dig more into this !!

While merging happens on the latest commits of both branches, rebasing requires rewriting history: First a `head rewound` where GIT moves the HEAD pointer back in time, and then `applying commit` where each commit of the branch is being cherry-picked to the new base.

!! I don't completely understand rebasing yet, so have to dig more into this !!



<br><br>

////

Stuff to be cleaned still:





### Different resets

`$ git reset –soft paste_haschode_here`: Reset to specified commit, but will keep changes in the staging area  
`$ git reset paste_haschode_here`: Default setting. Reset to specified commit, changes are in the working directory rather than the staging area  
`$ git reset –hard paste_haschode_here`: Really don’t want the changes made, I want to go back to where we were before. All files will match the state they were at the previous commit. Clears staging area and working directory on tracked files.  

<br>



## Miscellaneous:

Resetting to 300 minutes ago:
`$ git reset --hard master@{"300 minutes ago"}` [(Source)](https://twitter.com/data_stephanie/status/968226587547258886)

<br><hr>

## Resources

+ [Atlassian GIT tutorials](https://www.atlassian.com/git/tutorials/git-stash)

+ [Git in practice, by Mike McQuaid](https://github.com/GitInPractice/GitInPractice#readme)

+ [Youtube tutorials by Corey Schafer](https://www.youtube.com/watch?v=HVsySz-h9r4&list=PL-osiE80TeTuRUfjRe54Eea17-YfnOOAx)
