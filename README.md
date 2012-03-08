# Mathetes - IRC Bot

## Installing

### Ubuntu Prerequisites

Tested on Ubuntu 10.04.

    sudo su -
    apt-get install ruby ruby-dev libopenssl-ruby libpq-dev libxml2 libxml2-dev libxslt1.1 libxslt1-dev 

Install the latest rubygems (not from the repository).  Visit rubygems.org and download the latest and then run...

    ruby setup.rb

### Plugin Prerequisites

Clone the repository:

    git clone git://github.com/sag47/Mathetes.git

Pull in some requirements:

[silverplatter-irc](http://github.com/apeiros/silverplatter-irc)

    git clone git://github.com/sag47/silverplatter-irc.git

[silverplatter-log](git://github.com/apeiros/silverplatter-irc.git)

    git clone git://github.com/sag47/silverplatter-log.git

The plugins have dependencies:

    gem install eventmachine
    gem install mash -v 0.0.3
    gem install httparty -v 0.4.3
    gem install m4dbi
    gem install dbd-pg
    gem install nokogiri

### Set up memo plugin

Install postgresql and import the database.

    apt-get install postgresql
    su - postgres -c 'psql -U postgres -d postgres'

Change the password for postgres user in the psql database and create a new user and database for memo.  The following are SQL commands for psql after logging into the database as the postgres user.

    \password postgres
    CREATE USER memo;
    \password memo
    ALTER USER memo WITH CREATEDB;
    \q

Now we need to import the database (not SQL any more).

    psql -h localhost -U memo -d postgres < schema-memo.sql

## Configuring

    cp mathetes-config.yaml.sample mathetes-config.yaml

You'll want to change things like nick, password and channels for starters.

If you're going to use the Twitter plugin you also need to configure it:

    cp mathetes-twitter.yaml.sample mathetes-twitter.yaml

## Running

    ruby -rubygems -Ipath/to/silverplatter-irc/lib -Ipath/to/silverplatter-log/lib -Ilib irc-bot.rb

## Debugging

    ruby -vvvv -rdebug -rubygems -Ipath/to/silverplatter-irc/lib -Ipath/to/silverplatter-log/lib -Ilib irc-bot.rb

If you're using the mathetes service script then you can uncomment the line for debugging and comment out the line for normal running in run.sh.

## Ubuntu service integration and autostarting

Create a user that mathetes can use the client in separate from your normal user.

    adduser mathetes

Make sure the Mathetes directory is owned by user mathetes.

    chown -R /path/to/Mathetes

Link the service (sometimes it's more useful to make the link name the name of the irc bot)

    ln -s /path/to/mathetes.service /etc/init.d/mathetes

Autostart the service (mathetes is the name of the link in /etc/init.d/).  Also you should start the mathetes service after the PostgreSQL service (these commands will do that).

    update-rc.d mathetes start 30 2 3 4 5 . stop 30 0 6 .

If you ever need to stop the service from autostarting you can run the following.

    update-rc.d -f mathetes remove

You can now control the script with the service command.

    service mathetes help
    service mathetes start
    service mathetes stop

Sometimes you need to tell PostgreSQL to autostart.  Your service name might be different than mine.

    update-rc.d postgresql-8.4 start 19 2 3 4 5 . stop 21 0 1 6 .

## Support

Come visit me (Pistos) on FreeNode in the #mathetes channel, or report issues at
http://github.com/Pistos/Mathetes/issues .
