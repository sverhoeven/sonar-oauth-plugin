Sonar OAuth Plugin
=====================

[![Build Status](http://ns395624.ip-176-31-120.eu:8080/jenkins/job/Sonar%20OAuth%20Plugin/badge/icon)](http://ns395624.ip-176-31-120.eu:8080/jenkins/job/Sonar%20OAuth%20Plugin/)

Configuration
-------------

To `config/sonar.properties` add:

    sonar.security.realm=oauth
    sonar.oauth.providerId=github
    sonar.github.clientId.secured=**************
    sonar.github.clientSecret.secured=*****************
    sonar.oauth.adminUsers=<github user with sonar admin rights>


The Github provider adds a user to SonarQube groups which are the users Github organizations and teams:

* organization - a specific GitHub organization. You have to be a public member of the organization for the authorization to work correctly.
* organization*team - a specific GitHub team of a GitHub organization. Notice that organization and team are separated by an asterisk (*).

Groups are not created automaticly. So to have a user join a group, the group must exist first.

For a setup with mostly public projects and some private projects do:

1. Create a group (eg. org1*admins) which corresponds to a Github team which should have admin rights
2. Create a group (eg. org1*project1) which corresponds to a Github team
3. Create template called 'private' which only allows admin (eg. org1*admins) group
4. Create template called 'public' which allow anonymous access
5. Create a template (eg. project1) which only allow a team (eg. org1*project1) access
6. Make the 'private' template the default

Now when a new project analysis is run, it will not show up for anonymous users.
A admin user can apply the 'public' template to make the project public or apply a team (eg. project1) template to make a project only accesable by the team members.

Currently this plugin does not allow technical local users to login using local username/password instead of oauth redirect. Setting `sonar.security.localUsers` does not work because oauth uses redirects instead of username/password submitted to sonar directly.