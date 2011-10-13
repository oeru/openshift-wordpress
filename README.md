Wordpress on OpenShift Express
==============================

This git repository helps you get up and running quickly w/ a Wordpress installation
on OpenShift Express.  The backend database is MySQL and the database name is the 
same as your application name (using $_ENV['OPENSHIFT_APP_NAME']).  You can name
your application whatever you want.  However, the name of the database will always
match the application so you might have to update .openshift/action_hooks/build.


Running on OpenShift
----------------------------

Create an account at http://openshift.redhat.com/

Create a php-5.3 application (you can call your application whatever you want)

    rhc-create-app -a wordpress -t php-5.3

Add MySQL support to your application

    rhc-ctl-app -a wordpress -e add-mysql-5.1

Add this upstream Wordpress repo

    cd wordpress 
    git remote add upstream -m master git://github.com/openshift/wordpress-example.git
    git pull -s recursive -X theirs upstream master
    # note that the git pull above can be used later to pull updates to Wordpress
    
Then push the repo upstream

    git push

That's it, you can now checkout your application at (default admin account is admin/admin):

    http://wordpress-$your_domain.rhcloud.com


NOTES:

GIT_ROOT/.openshift/action_hooks/build:
    This script is executed with every 'git push'.  Feel free to modify this script
    to learn how to use it to your advantage.  By default, this script will create
    the database tables that this example uses.

    If you need to modify the schema, you could create a file 
    GIT_ROOT/.openshift/action_hooks/alter.sql and then use
    GIT_ROOT/.openshift/action_hooks/build to execute that script (make susre to
    back up your application + database w/ rhc-snapshot first :) )

Wordpress Security:
    If you're doing more than just 'playing' be sure to edit wp-config.php and modify
    the Authentication Unique Keys and Salts.  You can use the Wordpress site auth
    key generator @ https://api.wordpress.org/secret-key/1.1/salt to help.

