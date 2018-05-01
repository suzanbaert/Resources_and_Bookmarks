# Part 2: Fixing your screw-ups

## Exploring history

Useful commands:
+ `$ git log`: shows all latest logs. can take additional arguments for timestamps, regex, etc...
+ `$ git log --oneline`: shows all commits on one oneline
+ `$ git log --oneline --graph`: shows a graphical representation including branching
+ `git log --follow file.ext`: shows version history of a file (including renames if done through git)
+ `$ git reflog`: history of all times that the HEAD pointer changes, so this log includes checking out branches, and git resets. This contains only the local changes though, no what others did.

+ `$ git show hashcode`: shows the information of a certain commit
+ `$ git diff hashcode1 hashcode2`: checking differences in commits

Whenever you get into merge conflict windows, type the message and then hit `ESC` + `:wq` to exit.


On the different types of resets:
+ `$ git reset <ref>`: Default setting. Reset to specified commit, changes are in the working directory rather than the staging area.  
+ `$ git reset -–soft <ref>`: Reset to specified commit, but will keep changes in the staging area  
+ `$ git reset -–hard <ref>`: Reset to specified commit. Clear working directory and staging area so all later work is gone.


<br><hr>

## Case 1: It's not staged or committed

**1. I want to get rid of changes I made in my working directory**

To get rid of changes on one file: reset it to the version currently committed.
```
$ git reset --hard file.ext

--or:

$ git checkout file.ext
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

+ `git reset file.ext`: will unstage this particular file (with edits)

+ `$ git reset`: will unstage the files and bring them back (with the edits inside) to the working directory. This reset uses the default `git reset --mixed`.

+ `git reset --hard`: will unstage files AND undo the changes made in them. (!)


<br>

## Case 3: When it's just been committed
This refers to your last commit.

**1. Changing your commit message**

```
$ git commit --amend -m "new commit message"
```

A commit message window will show, change your commit message and press `ESC` followed by `:wq`.


**2. Add something to the latest commit**

A file that wasn't saved, or a quick typo that you've spotted. Make the change needed, and then add to the previous commit.
```
$ git add -A
$ git commit --amend
```

Note: git commit --amend does change the history and gives a new hashcode so it should not be used when collaborating on repos after it has been pushed to other people.

Secondly, if you've pushed it, the next push needs to be forced as the master/origin already has that commit: `git push origin +master`.  
Or an easier alternative is using a soft reset (see point 3) - which will put everything back into the working directory, so you can add and re-commit.


**3. Undoing the latest commit (but keeping changes)**

To undo a commit and bring all changes back to the working directory (to do this on just a file, add the filename).
```
$ git reset --soft HEAD^1
```

**4. Undoing the latest commit (and throw away changes)**

Git revert will reset to the previous commit without erasing the commit-to-be-undone from the history.  
Find the hashcode from the latest commit:  

```
$ git revert hashcode-to-be-undone
```

Alternative is to reset a commit, which does mean it will be erased from history - usually bad for collaborative code. This will checkout the last but one commit, undoing the last one.

```
$ git reset HEAD^`
```


<br>

## Case 4: Going back to a previous commit (not the last one):

Easiest is to go back to a version of a previous file:  
This will revert that particular file into the old version in the working directory. To keep working on the old file, commit again. To exit the old file and return to the previous, do a hard reset.
```
$ git checkout hashcode file.ext
```

Checking out a full repo will result in `detached head state`, as the head pointer is now pointing to a commit, rather than a branch.
This allows to look around and going back to what you were working on by `git checkout master`.

```
$ git checkout hashcode
```

If you want to work from the checkout-commit, you need to direct the detached head to a new branch, to avoid creating orphans.
```
$ git checkout -b branchname
```
or in one go:

```
$ git checkout -b NewBranch hashcode
```

<br>


## Special case: I committed on master instead of my branch

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

While merging happens on the latest commits of both branches, rebasing requires rewriting history: First a `head rewound` where GIT moves the HEAD pointer back in time, and then `applying commit` where each commit of the branch is being cherry-picked to the new base.


<br><br>





## Miscellaneous:

Resetting to 300 minutes ago:  
`$ git reset --hard master@{"300 minutes ago"}` [(Source)](https://twitter.com/data_stephanie/status/968226587547258886)

Getting rid of remote branch that no longer exists:  
`$ git remote prune origin`



Always use `git push --force-with-lease` rather than `git push -f`. The former will fail if there are upstream changes you have not pulled, the latter will overwrite them. ([Jim Hester's advice](https://twitter.com/jimhester_/status/991288807076122625))


<br><hr>



## Resources

+ [Git in practice, by Mike McQuaid](https://github.com/GitInPractice/GitInPractice#readme)

+ [Atlassian GIT tutorials](https://www.atlassian.com/git/tutorials/git-stash)

+ [Youtube tutorials by Corey Schafer](https://www.youtube.com/watch?v=HVsySz-h9r4&list=PL-osiE80TeTuRUfjRe54Eea17-YfnOOAx)
