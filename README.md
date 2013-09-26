

Steps to Mavenize DVN



Created 8/12/2013	Kevin Condon, adapted from Akio's v3.3 Maven project

Update 9/26/2013      Kevin Condon, updated dependencies, added resources step,
changed pom file instructions    to use the finalized ones available from this
project rather than beginning with Akio's.



Introduction:



This updated dvn maven project only slightly modifies the work done previously
by Akio for v3.3: https://github.com/akio-sone/dvn-maven It updates the project
for use with the current DVN version under development, v3.6, eliminates most of
the modules so that only a DVN-root parent project and DVN-web module are left.



The goal was to have only a single DVN-web project but we found an issue with
installing private libraries to the local repository so instead rely on the
parent project DVN-root to do that.



To make use of this project, simply clone the repo and build in NetBeans or from
the command line. The steps to  Mavenize DVN are provided as reference.



I. Create project structure

II. Copy finalized pom files

III. Copy files from current Ant structure to Maven structure

IV. Update dependencies

V. Create local_libs, remote repository, file system structure

VI. Test build and deploy



I. Create Project Structure

Maven has a standardized directory structure and to facilitate producing a
single module, DVN-web, code needs to be consolidated under the DVN-web project.



1.  Create a maven project directory, eg. maventest

2.  Create a parent maven project DVN-root

-cd maventest

-mvn archetype:generate

-filter list, type 'pom-root'

-choose 1

-select latest version, 1.1, choose 3

-type groupId: edu.harvard.iq.dvn

-type artifactId: DVN-root

-type version: 3.6

-choose package, hit return to select default

-upon review of options, hit return to select default

1.  Double check directory and pom file is created: sub directory DVN-root now
    exists and pom.xml is the only content.

2.  Create a sub project, DVN-web

-cd DVN-root

-mvn archetype:generate

-filter list, type 'webapp-javaee6'

-choose 1: org.codehaus.mojo.archetypes:webapp-javaee6

-select the latest, 1.5, choose 8 or hit return for default

-type groupId: edu.harvard.iq.dvn

-type artifactId: DVN-web

-type version: 3.6

-choose package, hit return to select default

-upon review of options, hit return to select default

1.  Double check directory and pom file is created: sub directory DVN-web now
    exists and it contains /src and pom.xml.



Note: This step was mainly for creating the project structure. For expediency,
we will later copy the finalized pom.xml files in this repository from the
DVN-root and DVN-web projects.



II. Copy finalized pom files



Much of the work in converting a project to Maven involves identifying
dependencies. We originally used Akio's pom files for the DVN-root and DVN-web
projects as a baseline and modified as needed.

Since there are a number of changes to Akio's original pom files, we recommend
just using the finalized pom.xml files from this repository rather than going
through the process of updating the original files.



-preserve the generated pom files for reference, name them pom.xml.ORIG

-copy the DVN-root and DVN-web pom.xml files from this repository to the
DVN-root and DVN-web directories of the target project.

-update version at the top of each pom file to the current version.



III. Copy files from current Ant structure to Maven structure



Note: these instructions for copying source files are taken directly from Akio's
v3.3 project:



From Akio's readme, Step 4: DVN-web project:



(1) remove automatically created java files such as App.java and AppTest.java in
dvn directory (under main and test directories) and index.jsp from
../DVN-root/DVN-web/src/main/webapp



(2) copy source trees from DVN-EJB project:



(2-1) copy the two source sub-trees of DVN-EJB project (core and ingest under
../src/DVN-EJB/src/java/edu/harvard/iq/dvn) to
../DVN-root/DVN-web/src/main/java/edu/harvard/iq/dvn



(2-2) if it is still used, copy a non-edu package of DVN-EJB project (lia under
../src/DVN-EJB/src/java) to ../DVN-root/DVN-web/src/main/java/



(2-3) copy mime.types and services directory in ../src/DVN-EJB/src/java/META-INF

to ../DVN-root/DVN-web/src/main/java/META-INF



(2-4) copy persistence.xml in ../src/DVN-EJB/src/conf

to  ../DVN-root/DVN-web/src/main/java/META-INF



(3) copy two subdirectories (api and core) under
/src/DVN-web/src/edu/harvard/iq/dvn

to ../DVN-root/DVN-web/src/main/java/edu/harvard/iq/dvn



(3-1) move 3 files (dvn_helper.R, R2HTML.css, and README) from
../DVN-root/DVN-web/src/main/java/edu/harvard/iq/dvn/core/web/subsetting to
../DVN-root/DVN-web/src/main/resources/edu/harvard/iq/dvn/core/web/subsetting

(updated step, KC)



(4)copy properties files in ../src/DVN-web/src

to ../DVN-root/DVN-web/src/main/java



(5) copy all files and directories under ../src/DVN-web/web

to ../DVN-root/DVN-web/src/main/webapp



(updated step, KC)

Note: we will be copying this repository's finalized pom files so the remaining
steps in (6) are for reference.



(6) modify the pom.xml



DVN-web pm.xml:



(6-1) add the following parent-tag block to the pom.xml



\<parent\>

\<artifactId\>DVN-root\</artifactId\>

\<groupId\>edu.harvard.iq.dvn\</groupId\>

\<version\>3.6\</version\>

\</parent\>



(6-2) add the following repository tags to the pom.xml of the project. Icefaces
and seams jars are not available from the maven central repository:



\<repositories\>

\<repository\>

\<id\>jboss-public-repository-group\</id\>

\<name\>JBoss Public Maven Repository Group\</name\>

\<url\>http://repository.jboss.org/nexus/content/groups/public\</url\>

\</repository\>



\<repository\>

\<id\>jboss-releases\</id\>

\<name\>JBoss Release Repository\</name\>

\<url\>https://repository.jboss.org/nexus/content/repositories/releases\</url\>

\</repository\>

\<repository\>

\<url\>http://anonsvn.icefaces.org/repo/maven2/releases/\</url\>

\<id\>icefaces2-core\</id\>

\<layout\>default\</layout\>

\<name\>Repository for library ICEfaces Core (2.0.2)\</name\>

\</repository\>

\<repository\>

\<id\>ICEfaces Repo\</id\>

\<name\>ICEfaces Repo\</name\>

\<url\>http://anonsvn.icefaces.org/repo/maven2/releases/\</url\>

\</repository\>

\</repositories\>



(6-3) add the following dependency tags to the pom.xml

[see the pom.xml]



(6-4) add the following pom fragment under build tag of the pom.xml



\<resources\>

\<resource\>

\<directory\>src/main/resources\</directory\>

\</resource\>

\<resource\>

\<directory\>src/main/java\</directory\>

\<includes\>

\<include\>\*.properties\</include\>

\<include\>META-INF/\*.\*\</include\>

\<include\>META-INF/services/\*.\*\</include\>

\</includes\>

\</resource\>

\</resources\>



IV. Update dependencies



v3.6 has added a number of libraries that were not present in v3.3. Namely,
libraries supporting the SWORD api and soon to be added EZID functionality.



-Add the following dependencies to the bottom of the dependencies section of the
DVN-web pom.xml file



\<!-- non-public SWORD API jars --\>

\<dependency\>

\<groupId\>org.swordapp.server\</groupId\>

\<artifactId\>sword2-server\</artifactId\>

\<version\>1.0-classes\</version\>

\</dependency\>

\<dependency\>

\<groupId\>org.apache.abdera\</groupId\>

\<artifactId\>abdera-core\</artifactId\>

\<version\>1.1.1\</version\>

\</dependency\>

\<dependency\>

\<groupId\>org.apache.abdera\</groupId\>

\<artifactId\>abdera-i18n\</artifactId\>

\<version\>1.1.1\</version\>

\</dependency\>

\<dependency\>

\<groupId\>org.apache.abdera\</groupId\>

\<artifactId\>abdera-parser\</artifactId\>

\<version\>1.1.1\</version\>

\</dependency\>

\<dependency\>

\<groupId\>org.apache.axiom\</groupId\>

\<artifactId\>axiom-api\</artifactId\>

\<version\>1.2.10\</version\>

\</dependency\>

\<dependency\>

\<groupId\>org.apache.axiom\</groupId\>

\<artifactId\>axiom-impl\</artifactId\>

\<version\>1.2.10\</version\>

\</dependency\>

\<dependency\>

\<groupId\>org.apache.commons\</groupId\>

\<artifactId\>commons-compress\</artifactId\>

\<version\>1.5\</version\>

\</dependency\>

\<dependency\>

\<groupId\>commons-fileupload\</groupId\>

\<artifactId\>commons-fileupload\</artifactId\>

\<version\>1.2.1\</version\>

\</dependency\>

\<dependency\>

\<groupId\>xom\</groupId\>

\<artifactId\>xom\</artifactId\>

\<version\>1.1\</version\>

\</dependency\>



\<!-- non-public EZID jar --\>

\<dependency\>

\<groupId\>edu.ucsb.nceas.ezid\</groupId\>

\<artifactId\>ezid\</artifactId\>

\<version\>0.9.0\</version\>

\</dependency\>

\<dependency\>

\<groupId\>org.apache.commons.codec\</groupId\>

\<artifactId\>commons-codec\</artifactId\>

\<version\>1.6\</version\>

\</dependency\>

\<dependency\>

\<groupId\>org.apache.http.client.fluent\</groupId\>

\<artifactId\>fluent-hc\</artifactId\>

\<version\>4.2.5\</version\>

\</dependency\>

\<dependency\>

\<groupId\>org.apache.http.impl.client\</groupId\>

\<artifactId\>httpclient\</artifactId\>

\<version\>4.2.5\</version\>

\</dependency\>

\<dependency\>

\<groupId\>org.apache.http.client.cache\</groupId\>

\<artifactId\>httpclient-cache\</artifactId\>

\<version\>4.2.5\</version\>

\</dependency\>

\<dependency\>

\<groupId\>org.apache.http\</groupId\>

\<artifactId\>httpcore\</artifactId\>

\<version\>4.2.4\</version\>

\</dependency\>

\<dependency\>

\<groupId\>org.apache.http.entity.mime\</groupId\>

\<artifactId\>httpmime\</artifactId\>

\<version\>4.2.5\</version\>

\</dependency\>



\<!-- non-public fits.jar --\>

\<dependency\>

\<groupId\>nom.tam.fits\</groupId\>

\<artifactId\>fits\</artifactId\>

\<version\>2012-10-25-generated\</version\>

\</dependency\>



\<!-- non-public xalan.jar is --\>

\<dependency\>

\<groupId\>org.apache.xalan\</groupId\>

\<artifactId\>xalan\</artifactId\>

\<version\>2.4.1\</version\>

\</dependency\>



\<!-- non-public for network data ingest --\>

\<dependency\>

\<groupId\>org.sqlite\</groupId\>

\<artifactId\>sqlite-jdbc\</artifactId\>

\<version\>3.6.16\</version\>

\</dependency\>



V. Create local_libs, remote repository, file system structure



Note: These steps have already been completed and are listed for reference only.
Instead, use the finalized pom files from this repository and the local_libs
directory from this repository.

(updated step, KC)



In both Akio's and this maven project, there are non public, often 3rd party
jars that cannot be found on maven repositories and so must be stored locally.
Akio addresses this by using the maven exec plugin and running mvn
install:install-file in the DVN-root project to cache these in the local
repository.



We initially took this approach but it runs with every build. So instead we
created a local, "remote" repository on the file system, made reference to it in
the pom file, and it caches to the local repository only once.



-create a directory for the remote repository under the DVN-web directory, named
local_lib

-copy all jar files from the various DVN libraries to the local_lib directory

-build the unf, ingest, and network_utils projects separately and copy their
resulting jar files here as well.

-for each non-public jar, use the information from the dependency section to run
mvn deploy:deploy-file and add it to the repo at local_lib

example:

cd DVN-web

mvn deploy:deploy-file -DgroupId=ORG.oclc -DartifactId=harvester2
-Dversion=2007-05-31-generated -Dpackaging=jar
-Dfile=/maventest/DVN-root/DVN-web/local_lib/dvn-lib-COMMON/harvester2.jar
-DrepositoryId=dvn.private
-Durl=file:///Users/kevincondon/NetBeansProjects/maventest/DVN-root/DVN-web/local_lib

-add the following repository to the DVN-root pom.xml file:

\<repository\>

\<id\>dvn.private\</id\>

\<name\>DVN Repository hosting private jars\</name\>

\<url\>file:///maventest/DVN-root/DVN-web/local_lib\</url\>

\</repository\>

-remove all references to mvn exec plugin or mvn install from the plugins
section of the DVN-root pom file.



VI. Test build and deploy



-From NetBeans, choose the DVN-root project, select clean and build. The first
build fetches all dependencies so it may take a while, approximately 6 minutes.
Subsequent builds can be as fast as 9 seconds.

-To Deploy, select the DVN-web project and then run. Make sure the glassfish
server is running. You may have to select the web server for the project first
under properties, run, server.



If you encounter any trouble building, it is most likely a dependency issue.
Resolve the first dependency mentioned, try again, etc.



You can also build from the command line. Cd to the top level project directory,
maventest, mvn package






