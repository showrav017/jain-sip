# Credential Pre-Requisites #

Make sure you have followed the steps here to create your own gpg key http://www.sonatype.com/people/2010/01/how-to-generate-pgp-signatures-with-maven/

Also access for a new user should be requested at https://issues.sonatype.org/browse/OSSRH-3233 or https://issues.sonatype.org/browse/OSSRH-3198

# Perform a Stable Release #

```
cd m2/jain-sip-ri

mvn release:prepare

mvn release:perform
```

This last step will fail because jain sip is not following the maven conventions, the poms are located in m2 repository, so to continue the release do

```
cd target/checkout
git checkout <tag name>
cd m2/jain-sip-ri
mvn deploy -P release-sign-artifacts -Dgpg.passphrase=<mypassphrase>
```

Then go to https://oss.sonatype.org/index.html#welcome and login.
Click on Staging Repositories on the left hand side of the screen and close your repo then Release it.

Then
```
cd ../../../..
mvn release:clean
```

## Troubleshooting ##

If everything went well with mvn release:prepare but something failed during mvn release:perform or the maven artifacts aren't valid on oss.sonatype.org. You may need to redo the entire maven release process ie going back to the revision before you started the release and remove the tag.

Here is how to delete a tag from a remote Git repository.

If you have a tag named '12345' then you would just do this:

```
git tag -d 12345
git push origin :refs/tags/12345
```

That will remove '12345' from the remote repository.

Rename your current master branch:

```
git branch -m trunk-after-bad-release
```

Check out your good commit:

```
git checkout <revision>
```

Make your new master branch here:

```
git checkout -b master
```

Now you still have your current trunk around if you want to look at it later, but your master branch is back at your last known good point, ready to be added to. If you really want to throw away your trunk-after-bad-release, you can use:

```
git branch -D trunk-after-bad-release
```

You need to push the changes back to remote google code repo as well with

```
git push -f origin master
```


# Perform a Snapshot Release #

```
cd m2/jain-sip-ri

mvn deploy -P release-sign-artifacts -Dgpg.passphrase=<mypassphrase>
```