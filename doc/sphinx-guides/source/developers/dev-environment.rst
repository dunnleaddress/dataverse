=======================
Development Environment
=======================

.. contents:: :local:

Assumptions
-----------

This guide assumes you are using a Mac but we do have pages for :doc:`/developers/windows` and :doc:`/developers/ubuntu`. New users are advised to follow the procedure in below instruction in the order for a successful dev setup.

Requirements (make sure you install these before dev setup.)
------------------------------------------------------------

Dataverse
~~~~~~~~~

This instruction targets Dataverse version 4.2.1 and Mac.
I found the combination that works -> GlassFish 4.1 comes with NB 8.0.2 - Java 7 JDK - Mac El Capitan - EnterpriseDB postsql 9.3 (comes with pgadmin nice interface) - and DataVerse 4.2.1.

Project Architecture
~~~~~~~~~~~~~~~~~~~~

Generally speaking, DataVerse's architecture implementation including: Jave web-application dataverse.war deployed on Glassfish, PosgreSQL, and Solr for searching. The installation script will configure all the project components and connect them providing you supply the correct setup information and RESPECT the assumptions in this instruction.

How these components are connected under the hook i don't know :( yet ask @pdurbin . If you read the install script it will tell.

"@pdurbin I really want to describe the all parts of the project connected and communicate to each other but i can not do this, can you please briefly explain it here? Thanks!"

When download and install softwares use default settings unless specified.

Java
~~~~

Dataverse is developed on Java 7. An upgrade to Java 8 is being tracked at https://github.com/IQSS/dataverse/issues/2151

The use of Oracle's version of Java is recommended, which can be downloaded from http://www.oracle.com/technetwork/java/javase/downloads/index.html

Java 7 Archive download: http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html
(or google : Java SE Development Kit 7 - Downloading this 'Mac OS X x64	196.89 MB  	jdk-7u79-macosx-x64.dmg').

The version of OpenJDK available from package managers from common Linux distributions such as Ubuntu and Fedora is probably sufficient for small changes as well as day to day development.

Glassfish
~~~~~~~~~

As a `Java Enterprise Edition <http://en.wikipedia.org/wiki/Java_Platform,_Enterprise_Edition>`_ 7 (Java EE 7) application, Dataverse requires an applications server to run.

Glassfish 4.1+ is required, which can be downloaded from http://glassfish.java.net (No need if you use Netbeans)

Recommend Glassfish version no more than 4.1. And this Glassfish comes with Netbeans, so when download Netbeans version is: https://netbeans.org/downloads/8.0.2/

PostgreSQL
~~~~~~~~~~

PostgreSQL 9.x is required and can be downloaded from http://postgresql.org

Because JDBC driver for Dataverse 4.2.1 only supports PostgreSQL 9.1 hence download is http://www.enterprisedb.com/products-services-training/pgdownload#osx (Installer version Version 9.1.19) - You can install PostgreSQL with homebrew alternatively, I found it easier with enterpriseDB, you will also get pgAdmin in enterpriseDB.

Solr
~~~~

Dataverse depends on `Solr <http://lucene.apache.org/solr/>`_ for browsing and search.

Solr 4.6.0 is the only version that has been tested extensively and is recommended in development. Download and configuration instructions can be found below (it comes with dataverse project). An upgrade to newer versions of Solr is being tracked at https://github.com/IQSS/dataverse/issues/456

curl
~~~~

A command-line tool called ``curl`` ( http://curl.haxx.se ) is required by the setup scripts and it is useful to have curl installed when working on APIs. (curl --help is installed by default on El Capitan)

brew
~~~~

You need brew to install jq so here is the command to install brew: (ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)") taken from source http://brew.sh/

jq
~~

A command-line tool called ``jq`` ( http://stedolan.github.io/jq/ ) is required by the setup scripts. The command is ``brew install jq`` (taken from https://stedolan.github.io/jq/download/)

If you are already using ``brew``, ``apt-get``, or ``yum``, you can install ``jq`` that way. Otherwise, download the binary for your platform from http://stedolan.github.io/jq/ and make sure it is in your ``$PATH`` (``/usr/bin/jq`` is fine) and executable with ``sudo chmod +x /usr/bin/jq``.

Recommendations
---------------

Mac OS X
~~~~~~~~

The setup of a Dataverse development environment assumes the presence of a Unix shell (i.e. bash) so an operating system with Unix underpinnings such as Mac OS X or Linux is recommended. (The `development team at IQSS <http://datascience.iq.harvard.edu/team>`_ has standardized Mac OS X.) Windows users are encouraged to install `Cygwin <http://cygwin.com>`_.

Netbeans
~~~~~~~~

While developers are welcome to use any editor or IDE they wish, Netbeans 8+ is recommended because it is free of cost, works cross platform, has good support for Java EE projects, and happens to be the IDE that the `development team at IQSS <http://datascience.iq.harvard.edu/team>`_ has standardized on. 

NetBeans can be downloaded from http://netbeans.org. Please make sure that you use an option that contains the Jave EE features when choosing your download bundle. While using the installer you might be prompted about installing JUnit and Glassfish. There is no need to reinstall Glassfish, but it is recommended that you install JUnit.

This guide will assume you are using Netbeans for development.

Additional Tools (optional)
~~~~~~~~~~~~~~~~

Please see also the :doc:`/developers/tools` page ( or http://guides.dataverse.org/en/latest/developers/tools.html ), which lists additional tools that very useful but not essential.

Setting up your dev environment (after downloading and installing requirements are met)
-------------------------------

SSH keys
~~~~~~~~

You can use git with passwords over HTTPS, but it's much nicer to set up SSH keys. https://github.com/settings/ssh is the place to manage the ssh keys GitHub knows about for you. That page also links to a nice howto: https://help.github.com/articles/generating-ssh-keys

From the terminal, ``ssh-keygen`` will create new ssh keys for you:

- private key: ``~/.ssh/id_rsa`` - It is very important to protect your private key. If someone else acquires it, they can access private repositories on GitHub and make commits as you! Ideally, you'll store your ssh keys on an encrypted volume and protect your private key with a password when prompted for one by ``ssh-keygen``. See also "Why do passphrases matter" at https://help.github.com/articles/generating-ssh-keys

- public key: ``~/.ssh/id_rsa.pub`` - After you've created your ssh keys, add the public key to your GitHub account.

Clone Project from GitHub
~~~~~~~~~~~~~~~~~~~~~~~~~

Before making commits, please read about our :doc:`/developers/branching-strategy` to make sure you commit to the right branch.

Determine Which Repo To Push To
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Developers who are not part of the `development team at IQSS <http://datascience.iq.harvard.edu/team>`_ should first fork https://github.com/IQSS/dataverse per https://help.github.com/articles/fork-a-repo/

Cloning the Project from Netbeans
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

From NetBeans, click "Team" then "Git" then "Clone". Under "Repository URL", enter the `"ssh clone URL" <https://help.github.com/articles/which-remote-url-should-i-use/#cloning-with-ssh>`_ for your fork (if you do not have push access to the repo under IQSS) or ``git@github.com:IQSS/dataverse.git`` (if you do have push access to the repo under IQSS). See also https://netbeans.org/kb/docs/ide/git.html#github

I found it is easier to clone with https so enter url address for example ``https://github.com/dunnleaddress/dataverse.git`` and your user name and password. Then select Master and 4.2.1 branches - the one you are interested in.

AND DO NOT CHANGE CLONE NAME "dataverse" that the name the install script will assume to use.

Cloning the Project from the Terminal
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If you prefer using git from the command line, you can clone the project from a terminal and later open the project in Netbeans.

If you do not have push access to https://github.com/IQSS/dataverse clone your fork:

``git clone git@github.com:[your GitHub user or organization]/dataverse.git``

If you do have push access to https://github.com/IQSS/dataverse clone it:

``git clone git@github.com:IQSS/dataverse.git``

Installing and Running Solr
~~~~~~~~~~~~~~~~~~~~~~~~~~~

A Dataverse-specific ``schema.xml`` configuration file (described below) is required.

Download solr-4.6.0.tgz from http://archive.apache.org/dist/lucene/solr/4.6.0/solr-4.6.0.tgz to any directory you like but in the example below, we have downloaded the tarball to a directory called "solr" in our home directory. For now we are using the "example" template but we are replacing ``schema.xml`` with our own. We will also assume that the clone on the Dataverse repository was retrieved using NetBeans and that it is saved in the path ~/NetBeansProjects.

- ``cd ~/solr``
- ``tar xvfz solr-4.6.0.tgz``
- ``cd solr-4.6.0/example``
- ``cp ~/NetBeansProjects/dataverse/conf/solr/4.6.0/schema.xml solr/collection1/conf/schema.xml``
- ``java -jar start.jar``

Please note: If you prefer, once the proper ``schema.xml`` file is in place, you can simply double-click "start.jar" rather that running ``java -jar start.jar`` from the command line. Figuring out how to stop Solr after double-clicking it is an exercise for the reader.

Once Solr is up and running you should be able to see a "Solr Admin" dashboard at http://localhost:8983/solr

Once some dataverses, datasets, and files have been created and indexed, you can experiment with searches directly from Solr at http://localhost:8983/solr/#/collection1/query and look at the JSON output of searches, such as this wildcard search: http://localhost:8983/solr/collection1/select?q=*%3A*&wt=json&indent=true . You can also get JSON output of static fields Solr knows about: http://localhost:8983/solr/schema/fields

Before running installer
~~~~~~~~~~~~~~~~~~~~~~~~

There is one more thing you must do before running the installer step below. in Netbeans IDE rightclick project namely "dataverse" under project tab, select build and Maven will run and build the project code. Maven is configured in project codes to garthers together all information and dependencies to build a war file (web application) and you should see the newly created war file such as "/Users/dung/NetBeansProjects/dataverse/target/dataverse-4.2.1.war". I hope you will get green "Build Success" message. The set up script will look for the build or war file in this NB IDE, hence we need this step.

Also, (it is important) right-click on dataverse project in NB ide select properties in "Run" under "Build" section make sure the "Context Path" field is empty so it is matching with the installer script. The installer script assumes the webapp is in the root directory of the webserver and not a subdirectory such as http://localhost:8080/mysubdir/. It should just be http://localhost:8080/ as indicated at the end of a successful script installation in the next step.

And making sure you have a valid smtp server address such as smtp.yahoo.com (@pdurbin, can we make this optional - leave empty?)

Run installer
~~~~~~~~~~~~~

"@pdurbin, I think we should also mention how the script helps configuring solr search with other components in here? Thanks!"

Once you install Glassfish and PostgreSQL, you need to configure the environment for the Dataverse app - configure the database connection, set some options, etc. We have a new installer script that should do it all for you. Again, assuming that the clone on the Dataverse repository was retrieved using NetBeans and that it is saved in the path ~/NetBeansProjects:

``cd ~/NetBeansProjects/dataverse/scripts/installer``

``./install``

The script will prompt you for some configuration values. It is recommended that you choose "localhost" for your hostname if this is a development environment. For everything else it should be safe to accept the defaults.

The script is a variation of the old installer from DVN 3.x that calls another script that runs ``asadmin`` commands. A serious advantage of this approach is that you should now be able to safely run the installer on an already configured system.

All the future changes to the configuration that are Glassfish-specific and can be done through ``asadmin`` should now go into ``scripts/install/glassfish-setup.sh``.

Shibboleth
----------

If you are working on anything related to users, please keep in mind that your changes will likely affect Shibboleth users. Rather than setting up Shibboleth on your laptop, developers are advised to simply add a value to their database to enable Shibboleth "dev mode" like this:

``curl http://localhost:8080/api/admin/settings/:DebugShibAccountType -X PUT -d RANDOM``

For a list of possible values, please "find usages" on the settings key above and look at the enum.

Now when you go to http://localhost:8080/shib.xhtml you should be prompted to create a Shibboleth account.

Make changes to source codes and test on Netbeans IDE
-----------------------------------------------------

- Rightclick on the dataverse project in Projects tab/window and select Run.
- ...

Rebuilding your dev environment:
-------------------------------

If you have an old copy of the database and old Solr data and want to start fresh, here are the recommended steps: 

- drop your old database
- clear out your existing Solr index: ``scripts/search/clear``
- run the installer script above - it will create the db, deploy the app, populate the db with reference data and run all the scripts that create the domain metadata fields. You no longer need to perform these steps separately.
- confirm you are using the latest Dataverse-specific Solr schema.xml per the "Installing and Running Solr" section of this guide
- confirm http://localhost:8080 is up
- If you want to set some dataset-specific facets, go to the root dataverse (or any dataverse; the selections can be inherited) and click "General Information" and make choices under "Select Facets". There is a ticket to automate this: https://github.com/IQSS/dataverse/issues/619

Production Deployment:
---------------------

@pdurbin can we deploy this on tomcat?

Misc:
----

Uninstall Netbeans:
~~~~~~~~~~~~~~~~~~~

For installations from Java EE 5 Tools Bundle
Go to the Finder and open the Applications window. Find the NetBeans executable you want to uninstall.
Control-click (or right-click) the executable and select "Show package contents". ...
Double-click the uninstaller icon and follow the instructions.

Uninstall PostgreSQL:
~~~~~~~~~~~~~~~~~~~~~

To remove the EnterpriseDB One-Click install of PostgreSQL 9.1:

Open a terminal window. Terminal is found in: Applications->Utilities->Terminal
Run the uninstaller:

sudo /Library/PostgreSQL/9.1/uninstall-postgresql.app/Contents/MacOS/installbuilder.sh
If you installed with the Postgres Installer, you can do:

open /Library/PostgreSQL/9.2/uninstall-postgresql.app
It will ask for the administrator password and run the uninstaller.

Remove the PostgreSQL and data folders. The Wizard will notify you that these were not removed.

sudo rm -rf /Library/PostgreSQL
Remove the ini file:

sudo rm /etc/postgres-reg.ini
Remove the PostgreSQL user using System Preferences -> Users & Groups.

Unlock the settings panel by clicking on the padlock and entering your password.
Select the PostgreSQL user and click on the minus button.
Restore your shared memory settings:

sudo rm /etc/sysctl.conf
That should be all! The uninstall wizard would have removed all icons and start-up applications files so you don't have to worry about those.
