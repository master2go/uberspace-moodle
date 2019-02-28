LimeSurvey is a free and open source survey web application.

Note

For this guide you should be familiar with the basic concepts of

PHP
MySQL
domains
License

LimeSurvey software is licenced under the GPLv2.

Prerequisites

We're using PHP in the stable version 7.1:

[isabell@stardust ~]$ uberspace tools version show php
Using 'PHP' version: '7.1'
[isabell@stardust ~]$
You'll need your MySQL credentials. Get them with my_print_defaults:

[isabell@stardust ~]$ my_print_defaults client
--default-character-set=utf8mb4
--user=isabell
--password=MySuperSecretPassword
[isabell@stardust ~]$
Your survey URL needs to be setup:

[isabell@stardust ~]$ uberspace web domain list
isabell.uber.space
[isabell@stardust ~]$
Installation

Download archive

Visit LimeSurvey's stable release page and copy the .tar.gz download link. Then, cd to your DocumentRoot and use wget to download the file. Make sure to replace the dummy URL with the one you just copied.

Extract archive

[isabell@stardust html]$ tar -xzf limesurvey.tar.gz --strip-components=1
[isabell@stardust html]$
Configuration

Point your browser to your domain (e.g. https://isabell.uber.space) and use the installer to set up your database and admin user account.

We recommend to use a new database such as isabell_limesurvey for LimeSurvey.

Edit .htaccess

The default .htaccess includes a RewriteCond so that existing directories won't be rewritten, but for some reason it is commented out:

#RewriteCond %{REQUEST_FILENAME} !-d
Edit the .htaccess file and uncomment the line above, so the full .htaccess file should look like this:

<IfModule mod_rewrite.c>
    RewriteEngine on

    # if a directory or a file exists, use it directly
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteCond %{REQUEST_FILENAME} !-d

    # otherwise forward it to index.php
    RewriteRule . index.php

    # deny access to hidden files and directories except .well-known
    RewriteCond %{REQUEST_URI} !^/\.well-known
    RewriteRule ^(.*/)?\.+ - [F]
</IfModule>

# deny access to hidden files and directories without mod_rewrite
RedirectMatch 403 ^/(?!\.well-known/)(.*/)?\.+

# General setting to properly handle LimeSurvey paths
# AcceptPathInfo on
Best practices

Updates

Note

Check the update feed regularly to stay informed about the newest version.

When a new version is released, copy the download link and download it as above, but exclude /application/config/config.php and /upload/* when extracting the archive.

Tested with LimeSurvey 3.14.11+180926, Uberspace 7.1.13.0

.. authors::
