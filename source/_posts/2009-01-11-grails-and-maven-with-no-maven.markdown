---
author: mjwall
comments: false
date: 2009-01-11 02:27:07+00:00
layout: post
slug: grails-and-maven-with-no-maven
title: Grails and Maven with no Maven
wordpress_id: 89
alias: /2009/01/grails-and-maven-with-no-maven/
categories:
- grails
- groovy
- java
- maven
---

According the [Grails 1.1 Beta2 Release Notes](http://www.grails.org/1.1-beta2+Release+Notes), Grails 1.1 will have better Maven integration.  I think that is great news allowing more integration between different java projects.  It means this post may not relevant for very long though.

For all it's complexity, I like maven.  Sometimes it makes complex tasks easier, but not always.  Here is my situation.  There are several java projects already using maven.  I am building a grails project that will be used by some of these projects.  The easiest integration is to package up a war file and deploy it to our local maven repository.  [Nexus](http://nexus.sonatype.org/) is great by the way.  The other projects can include my grails project as a dependency.  So what is the best way to do that?  Right, I hear you.  By hand.  However, I need to automate this.

I looked at the [grails-maven-plugin](http://forge.octo.com/maven/sites/mtg/grails-maven-plugin/), but it is too much.  I am not trying to mavenize my project, just deploy it to Nexus.

Luckily, there is as a cleaner answer.   Creating scripts in grails is [easy](http://docs.codehaus.org/display/GRAILS/Command+Line+Scripting).  Those scripts can use the [maven-ant-tasks](http://maven.apache.org/ant-tasks/index.html).  Download the [jar](http://www.apache.org/dyn/closer.cgi/maven/binaries/maven-ant-tasks-2.0.9.jar) file and put it in your lib directory.

I'll create 2 tasks, one handle the 'maven install' so I can test locally and one to handle 'maven deploy'.  We need a pom file, but instead of checking one in, let's generate it from the project so it picks up the latest version etc.  (Note, I studied the [gant](http://github.com/russel/gant/tree/master/build.gant) build file pretty closely).  Here is an example of MavenInstall.groovy file from the scripts directory



1 includeTargets << grailsScript ( "War" )
2
3 final antlibXMLns = 'antlib:org.apache.maven.artifact.ant'
4 final tempPomFile = "pom.xml"
5
6 target (preparePom : "Generate a temporary pom file") {
7  depends(packageApp)  _//so config.maven properties are loaded_
8  def writer = new StringWriter()
9  def builder = new groovy.xml.MarkupBuilder(writer)
10  builder.project(xmlns:"http://maven.apache.org/POM/4.0.0",
11  'xmlns:xsi':"http://www.w3.org/2001/XMLSchema-instance",
12  'xsi:schemaLocation':"http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd") {
13  'modelVersion' '4.0.0'
14  'groupId' 'com.mjwall'
15  'artifactId' grailsAppName
16  'packaging' 'war'
17  'version' grailsAppVersion
18  }
19
20  File temp = new File(tempPomFile)
21  temp.write writer.toString()
22  def pom =ant."${antlibXMLns}:pom" ( file : tempPomFile , id : tempPomFile )
23  echo("Temporary pom file written to ${tempPomFile}, don't forget to clean up")
24  return tempPomFile
25 }
26
27 target (deletePom : "Clean up the temporary pom file") {
28  new File(tempPomFile).delete()
29 }
30
31 target (getWarName : "Return the war name defined by the app configs") {
32  depends(war)
33  _// bug in grails clean, doesn't seem to delete war from a custom location defined by grails.war.destFile_
34  return warName
35 }
36
37 target (default : "Install to local maven repository") {
38  depends(war)
39  def tempPom = preparePom()
40  ant."${antlibXMLns}:install" ( file : getWarName() ) { pom ( refid : tempPom ) }
41  deletePom()
42 }
43


Not too scary, but what is going on here?  The default task you will see depends on the war file being built.  A temporary pom is created using the value defined in application.properties.  Then the maven-ant-task for install is run and the pom is deleted.  Pretty simple huh?  It is once you see an example anyway.

How about a maven deploy.  Basically the same thing, except the pom needs to generate a distributionManagement section.  I set up a couple of properties in my Config.groovy at the end called maven.remoteReleaseUrl and maven.remoteSnapshotUrl.  Change the pom generate to include them.  Looks like this



6 target (preparePom : "Generate a temporary pom file") {
7  depends(packageApp)  _//so config.maven properties are loaded_
8  def writer = new StringWriter()
9  def builder = new groovy.xml.MarkupBuilder(writer)
10  builder.project(xmlns:"http://maven.apache.org/POM/4.0.0",
11  'xmlns:xsi':"http://www.w3.org/2001/XMLSchema-instance",
12  'xsi:schemaLocation':"http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd") {
13  'modelVersion' '4.0.0'
14  'groupId' 'com.mjwall'
15  'artifactId' grailsAppName
16  'packaging' 'war'
17  'version' grailsAppVersion
20  'distributionManagement' {
21  'repository' {
22  'id' 'releases'
23  'name' 'Internal Releases'
24  'url' config.maven.remoteReleaseUrl _//defined in Config.groovy_
25  }
26  'snapshotRepository' {
27  'id' 'snapshots'
28  'name' 'Internal Snapshots'
29  'url' config.maven.remoteSnapshotUrl _//defined in Config.groovy_
30  'uniqueVersion' 'true'
31  }
18  }
19
20  File temp = new File(tempPomFile)
21  temp.write writer.toString()
22  def pom =ant."${antlibXMLns}:pom" ( file : tempPomFile , id : tempPomFile )
23  echo("Temporary pom file written to ${tempPomFile}, don't forget to clean up")
24  return tempPomFile
25 }


Then instead of calling ant."${antlibXMLns}:install", call ant."${antlibXMLns}:deploy"

Couple of notes.  I did run into problems with my settings.xml file defined in ~/.m2, so I ended up creating one much like I did with the pom.xml.  Only used when deploying, so it is not built for the install.  Finally, because grails can only run one task per script, I DRYed up this whole thing with one file called MavenUtils that included all code for both install and deploy.  Then, my MavenInstall file just loads the MavenUtils and calls install.

Wow, more explanation than I thought it would.  And I guess the title is not entirely accurate.  I am using maven, but not by calling the maven executable directly.  Hopefully the new Maven/Grails integration will make this easier.  Fingers crossed.
