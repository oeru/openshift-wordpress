Wordpress on OpenShift
======================

This git repository helps you get up and running quickly w/ a Wordpress installation
on OpenShift.  The backend database is MySQL and the database name is the 
same as your application name (using $_ENV['OPENSHIFT_APP_NAME']).  You can name
your application whatever you want.  However, the name of the database will always
match the application so you might have to update .openshift/action_hooks/build.


Running on OpenShift
----------------------------

Create an account at http://openshift.redhat.com/

Create a php-5.3 application (you can call your application whatever you want)

    rhc app create -a wordpress -t php-5.3

Add MySQL support to your application

    rhc cartridge add -a wordpress -c mysql-5.1

Add this upstream Wordpress repo

    cd wordpress 
    git remote add upstream -m master git://github.com/openshift/wordpress-example.git
    git pull -s recursive -X theirs upstream master
    # note that the git pull above can be used later to pull updates to Wordpress
    
Then push the repo upstream

    git push

That's it, you can now checkout your application at:

    http://wordpress-$yournamespace.rhcloud.com
    
Default Credentials
-------------------
<table>
<tr><td>Default Admin Username</td><td>admin</td></tr>
<tr><td>Default Admin Password</td><td>OpenShiftAdmin</td></tr>
</table>

Notes
=====

GIT_ROOT/.openshift/action_hooks/deploy:
    This script is executed with every 'git push'.  Feel free to modify this script
    to learn how to use it to your advantage.  By default, this script will create
    the database tables that this example uses.

    If you need to modify the schema, you could create a file 
    GIT_ROOT/.openshift/action_hooks/alter.sql and then use
    GIT_ROOT/.openshift/action_hooks/deploy to execute that script (make sure to
    back up your application + database w/ 'rhc app snapshot save' first :) )

Security Considerations
-----------------------
This repository contains configuration files with security related variables.

Since this is a shared repository, any applications derived from it will share those variables, thus reducing the security of your application.

You should follow the directions below and push your updated files to OpenShift immediately.

### Procedure

The following table lists files and the procedure for securing.

<table>
  <tr>
    <th>File</th>
    <th>Directions</th>
  </tr>
  <tr>
    <td>php/wp-config.php</td>
    <th>http://codex.wordpress.org/Editing_wp-config.php#Security_Keys</th>
  </tr>
</table>
