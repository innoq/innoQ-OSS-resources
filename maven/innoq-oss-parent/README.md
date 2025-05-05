Releasing the Parent POM
========================

The release plugin assumes the POM is named pom.xml and lives in the
root of your git repository.  This seems to be an explicit design goal
with reasoning along the lines of "git is one repo per project and mvn
is POM at the root of the project".  Of course this doesn't help if
Java stuff is only part of the project ...

Given this really is just a simple pom, we can trivially perform the
steps manually.

Create a branch you want to hold your release and check it out.

Then

```
mvn versions:set -DnewVersion=WHATEVER-RELEASE-VERSION
git add pom.xml
git commit -m "release version WHATEVER-RELEASE-VERSION"
git tag -s -m "release version WHATEVER-RELEASE-VERSION" TAG-NAME
mvn clean deploy -Psonatype-central-release,release
mvn versions:set -DnewVersion=WHATEVER-NEXT-VERSION-SNAPSHOT
```

This should do the trick. Publish the release via
https://central.sonatype.com/publishing/deployments .

Comit, push the branch and tags, merge the branch to `master`.

