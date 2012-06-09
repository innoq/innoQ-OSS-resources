Releasing the Parent POM
========================

The release plugin assumes the POM is named pom.xml and lives in the
root of your git repository.  This seems to be an explicit design goal
with reasoning along the lines of "git is one repo per project and mvn
is POM at the root of the project".  Of course this doesn't help if
Java stuff is only part of the project ...

This means the usual

     $ mvn release:clean release:prepare
     $ mvn release:perform

cycle is not going to work, the perform step has to be done manually.
The process is 

     $ mvn clean release:clean release:prepare
     $ git clone git@github.com:innoq/innoQ-OSS-resources.git target/checkout
     $ cd target/checkout
     $ git checkout TAG-CHOSEN-IN-PREPARE-STEP
     $ cd maven/innoq-oss-parent
     $ cp pom.xml innoq-oss-parent-VERSION_OF_RELEASE.pom
     $ mvn gpg:sign-and-deploy-file -Durl=https://oss.sonatype.org/service/local/staging/deploy/maven2/ -DrepositoryId=sonatype-nexus-staging -DpomFile=innoq-oss-parent-VERSION_OF_RELEASE.pom -Dfile=innoq-oss-parent-VERSION_OF_RELEASE.pom

This should do the trick.  After that the target/checkout directory
can be removed.
