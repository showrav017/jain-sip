Just a reminder on commands used to create the repo and keeping it in sync :

```
git svn clone -s https://svn.java.net/svn/jsip~svn jain-sip
cd jain-sip/
git remote add origin https://code.google.com/p/jain-sip/
git svn rebase
git status
git push origin master
```

If the repository doesn't follow the SVN conventions one can do the initial cloning using the following command
```
git svn clone -Ttrunk/tools/maven-eclipse-plugin -bbranches/tools/maven-eclipse-plugin -ttags/tools/maven-eclipse-plugin https://mobicents.googlecode.com/svn maven-eclipse-plugin
```

#Updating from and committing back to Subversion
#Before committing back to Subversion, you will want to update to apply any new changes in the repository to your local Git repo.

```
git-svn rebase
```

#This will download all new changesets from Subversion, apply them to the last checkout from Subversion, and then re-apply your local changes on top of that. When you’re ready to commit back to Subversion, execute

```
git-svn dcommit
```

this jain-sip git repo as a branch called trunk on which all the work is done.

To work on it do a

```
git checkout trunk
```

Modify the files to fix an issue or work on a feature, when ready do

```
git commit -a (and potentially git add <files>) (can be done through egit)
```


to commit the files (it is recommended to do interim commits in git as you work on a long feature) then do a

```
git rebase trunk 
```

to get all the changes from trunk and make sure things are up to date, if there is a warning on stashing do

```
git stash
```

Then make sure everything is updated and in sync with SVN repo as well

```
git svn rebase trunk
```

Then commit your changes back to the SVN repo

```
git svn dcommit trunk
```

Then merge the changes back to master here

checkout the master branch

```
git checkout master
```

Merge the changes back from trunk to the current master branch
```
git merge trunk
```

Push changes to the repo
```
git push origin master 
```

Then ready to work back on the trunk

```
git checkout trunk
```

If you stashed previously make sure to unstash (more on unstashing at http://git-scm.com/book/en/Git-Tools-Stashing)

```
git stash apply
```

In case you messed up and want to revert to the latest HEAD any uncommitted change

```
git reset --hard HEAD
```

Note: You may also want to run

```
git clean -f 
```

as

```
git reset --hard
```

will not remove untracked files

To get update from SVN on a particular branch, just check out the branch with local name of the branch to create and remote name of the branch

```
git checkout -b git-JAIN-SIP-NIO-year-2012 remotes/JAIN-SIP-NIO-year-2012
```

To push this branch to the repository

```
git push origin git-JAIN-SIP-NIO-year-2012
```

To resync the repo to see new branches, use the following command
```
git svn fetch
```

You can get a list of all branches (remote and local) through the followning command

```
git branch -a
```

To get regular (non SVN) git updates on a particular branch

```
git pull --rebase origin trunk
```

(--rebase will automatically move your local commits, if you have any, on top of the latest branch you pull from, you can leave it off if you do not).

Please note that --rebase is very important if you do have commits. What happens is that when git pull can't fast forward, it does a merge commit, and a merge commit puts the sucked in changes ON TOP of yours whereas a rebase puts them BELOW yours. In other words a merge commit makes the history a graph, and we prefer a cleaner, easier to follow linear history (hence the rebasing). Further once you do a merge commit it will be difficult to rebase the history before that commit (say you want to combine two commits to one later) as described in point 13. Luckily the option set in step 5 will prevent this from happening.

One way to not forget --rebase the rebase option is you may want to create an alias

```
git config --global alias.up "pull --rebase"
```

and then just use the new alias instead of pull

```
git up origin trunk
```


In case you messed up pretty bad your local git svn syncing and it always tries to fast forward and commit a bunch of revisions that have already been committed over and over again. You can delete your current branch locally and remotely and create a new by tracking the remote trunk :

```
git branch -D master
git push origin :master

git checkout -b git-trunk remotes/trunk
git push origin git-trunk
```

To create a diff with untracked files not included in the diff you can run

```
git diff --ignore-submodules=dirty
```

You probably won't need to do this often (if ever at all) but just in case, here is how to delete a tag from a remote Git repository.

If you have a tag named '12345' then you would just do this:

```
git tag -d 12345
git push origin :refs/tags/12345
```

That will remove '12345' from the remote repository.

If you remove trunk or a branch that is the upstream and default branch when cloning or checking out through CI jenkins jobs with . You may have the wrong default branch to modify it and point back to master or another branch use the following

```
git branch --set-upstream master origin/master
```


In case you want to checkout to a branch that was pushed by another member

```
git remote update
git pull --all
git checkout -b <branch_name> origin/<branch_name>
```

Good articles

http://code.google.com/p/support/wiki/ExportingToGit

http://viget.com/extend/effectively-using-git-with-subversion

http://blog.emmanuelbernard.com/2010/05/git-how-my-life-has-improved-since-last-month-when-i-used-svn/

http://book.git-scm.com/4_undoing_in_git_-_reset,_checkout_and_revert.html

https://community.jboss.org/wiki/HackingOnAS7