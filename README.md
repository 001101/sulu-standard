Sulu [![Selenium Test Status](https://saucelabs.com/buildstatus/sulu-cmf)](https://saucelabs.com/u/sulu-cmf) [![](https://travis-ci.org/sulu-cmf/sulu-standard.png)](https://travis-ci.org/sulu-cmf/sulu-standard) [![Scrutinizer Code Quality](https://scrutinizer-ci.com/g/sulu-cmf/sulu-standard/badges/quality-score.png?s=3039e48d6515ea846578ca06f3c5bd5442ad3c5b)](https://scrutinizer-ci.com/g/sulu-cmf/sulu-standard/)
============================================================================================================

Welcome to Sulu - a fully-functional  Content Management Framework (CMF) based on Symfony2.

Licensed under the [MIT License](https://github.com/sulu-cmf/SuluContentBundle/blob/develop/LICENSE).

##Prerequisites

- webserver with PHP (>=5.4) and a mysql database
- [composer](https://getcomposer.org/)
- [grunt-cli](http://gruntjs.com/getting-started)
- [nodejs and npm](http://nodejs.org/)
- [ruby](https://www.ruby-lang.org/en/downloads/) & [compass](http://compass-style.org/install/) (3.2.13)


#### Configuration

Use the following template for your vhost-configuration
```
<VirtualHost *:80>
    DocumentRoot "[path-to-your-workspace]/sulu-standard/web"
    ServerName sulu.lo
    <Directory "[path-to-your-workspace]/sulu-standard/web">
        Options Indexes FollowSymlinks
        AllowOverride All
        Order allow,deny
        Allow from all
    </Directory>
</VirtualHost>
```

An extended example for Apache and Nginx can be found [here](https://github.com/sulu-cmf/docs/blob/master/developer-documentation/000-foundation/vhost.md).

Of course you also have to make sure that your webserver user has permission
to access your installation folder.  Don't forget to include `sulu.lo` in your
hosts-file, if you want to use Sulu on a local machine.


## Installation

#### Clone this repository

```
git clone git@github.com:sulu-cmf/sulu-standard.git
cd sulu-standard
```

#### Checkout the develop-branch

```
git checkout develop
```

##### Webspaces
Webspaces are configured in the `app/Resources/webspaces`-folder. Copying the existing example should be enough for a local installation:
```
cp app/Resources/webspaces/sulu.io.xml.dist app/Resources/webspaces/sulu.io.xml
```
On an online installation you have to adjust the URLs in this file.

##### Templates
Templates are configured in the `app/Resources/pages`-folder. Copying the existing default template should be enough for a simple page containing a title, a link and a texteditor:

```
cp app/Resources/pages/default.xml.dist app/Resources/pages/default.xml
cp app/Resources/pages/overview.xml.dist app/Resources/pages/overview.xml
cp app/Resources/pages/complex.xml.dist app/Resources/pages/complex.xml
```
You can add more templates by simply adding more files in this folder. Use the `default.xml.dist`-file as an example.

#### Snippets
Snippets are configured in the `app/Resources/snippets`-folder. Copy the existing default snippet-types.

```
cp app/Resources/snippets/default.xml.dist app/Resources/snippets/default.xml
```

You can add more snippet-types by simply adding more files in this folder. Use the `default.xml.dist`-file as an example.

#### Install all the dependencies with composer

```
composer install
```

Composer will ask for some parameters. The default values are good enough to get a running setup.

#### Clear the caches and set the appropriate permissions

##### Mac OSX
```
rm -rf app/cache/*
rm -rf app/logs/*
mkdir app/data
mkdir uploads/media
mkdir web/uploads/media
APACHEUSER=`ps aux | grep -E '[a]pache|[h]ttpd' | grep -v root | head -1 | cut -d\  -f1`
sudo chmod +a "$APACHEUSER allow delete,write,append,file_inherit,directory_inherit" app/cache app/logs uploads/media web/uploads/media app/data
sudo chmod +a "`whoami` allow delete,write,append,file_inherit,directory_inherit" app/cache app/logs uploads/media web/uploads/media app/data
```

##### Ubuntu
```
rm -rf app/cache/*
rm -rf app/logs/*
mkdir app/data
mkdir uploads && mkdir uploads/media/
mkdir web/uploads && mkdir web/uploads/media/*
sudo setfacl -R -m u:www-data:rwx -m u:`whoami`:rwx app/cache app/logs uploads/media web/uploads/media app/data
sudo setfacl -dR -m u:www-data:rwx -m u:`whoami`:rwx app/cache app/logs uploads/media web/uploads/media app/data
```

#### Build Sulu

The following command will intiaialize the database, load the fixtures and do
other things required to intiialize your Sulu CMF installation:

Be sure that you installed jackrabbit or doctrine-dbal (next section of instructions)

````
app/console sulu:build dev
````

#### Optional

##### Download and install Jackrabbit

Use Apache Jackrabbit as a content repository (as an alternative to
doctrine-dbal (i.e. storing the data in a relational database).

Download the jar file into a Folder of your choice.

```
wget http://archive.apache.org/dist/jackrabbit/2.6.3/jackrabbit-standalone-2.6.3.jar
java -jar jackrabbit-standalone-2.6.3.jar
```
##### Imagick - for better Image handling

###### Mac OSX
```
brew install imagemagick
brew install php55-imagick
```
add `extension=/path/to/Imagick.so` to `php.ini`

###### Ubuntu
```
sudo apt-get install imagemagick
sudo apt-get install php5-imagick
```

add `extension=/path/to/Imagick.so` to `php.ini`

##### GhostScript - PDF previews

###### Mac OSX
```
brew install ghostscript
```
configurate the path to `ghostscript` in app/config/parameters.yml under `ghost_script_path`

###### Ubuntu
```
sudo apt-get install ghostscript
```
configurate the path to `ghostscript` in app/config/parameters.yml under `ghost_script_path`

#### Create required configuration files
Before you go on with the initialization of the content repository, you have to make sure that all required configuration files exist.

##### Widgets

Widgets are small UI-components which are mainly seen in the sidebar. In the configuration file it can be specified which widgets are contained in which sidebar. To use the default configuration just run the follwing command:

```
cp app/config/widgets.yml.dist app/config/widgets.yml
```

#### Insert a new user
```
app/console sulu:security:user:create
```
Follow the instruction to create a new user


## What's inside?

- [admin-bundle](https://github.com/sulu-cmf/SuluAdminBundle)
- [contact-bundle](https://github.com/sulu-cmf/SuluContactBundle)
- [content-bundle](https://github.com/sulu-cmf/SuluContentBundle)
- [generator-bundle](https://github.com/sulu-cmf/SuluGeneratorBundle)
- [portal-bundle](https://github.com/sulu-cmf/SuluPortalBundle)
- [security-bundle](https://github.com/sulu-cmf/SuluSecurityBundle) (included but not integrated in other bundles)
- [sulu-core](https://github.com/sulu-cmf/sulu)
- [tag-bundle](https://github.com/sulu-cmf/SuluTagBundle)
- [test-bundle](https://github.com/sulu-cmf/SuluTestBundle)
- [website-bundle](https://github.com/sulu-cmf/SuluWebsiteBundle)

## Usage

The route to the backend login is:

```
sulu.lo/admin/
```

## Documentation

Documentation can be found here: [https://github.com/sulu-cmf/docs](https://github.com/sulu-cmf/docs)
