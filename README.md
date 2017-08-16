# AMP-on-MAC-OS-from-scratch

## install xcode properties (this is probably installed if your MAC is not new)
xcode-select --install

############################################ Install iTerm2
https://www.iterm2.com/
https://iterm2.com/downloads/stable/latest

############################################ Install oh-my-zshell
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
nano ~/.zshrc



############################################ Install brew
https://brew.sh/
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew doctor



############################################ Install php71
brew install homebrew/php/php71 --with-httpd24

sudo nano /usr/local/etc/apache2/2.4/httpd.conf

// uncomment these lines
#LoadModule rewrite_module libexec/mod_rewrite.so
#Include /usr/local/etc/apache2/2.4/extra/httpd-vhosts.conf

ServerName localhost
Listen 80

// set username and group
User mac
Group staff

// replace line: "DirectoryIndex index.html" with this ˇ
DirectoryIndex index.php index.html

// add to bottom
<FilesMatch .php$>
    SetHandler application/x-httpd-php
</FilesMatch>

Start your nternet browser and visit localhost
You should get a message "It works!"
which comes from index.html file in 
/usr/local/var/www/htdocs

// install xdebug for php debugging
brew install homebrew/php/php71-xdebug

############################################ Install mysql
brew install mysql


//check versions
apachectl -v
php -v
mysql -V


############################################ Make your first virtual host

// make a directory for your virtual host sites
mkdir -p /Users/yourusername/Documents/ApacheVhosts
sudo chgrp _www /Users/yourusername/Documents/ApacheVhosts
sudo g+s /Users/yourusername/Documents/ApacheVhosts

// create a folder with index.php file to test php and your server
mkdir -p /Users/yourusername/Documents/ApacheVhosts/phptest
nano /Users/yourusername/Documents/ApacheVhosts/phptest.local/index.php

copy/paste this into it:
<?php

phpinfo();

Ctrl+X, Y, Enter

// register the vhost in vhosts.config
sudo nano /usr/local/etc/apache2/2.4/extra/httpd-vhosts.conf

<VirtualHost *:80>

    ServerName servername.com

    DocumentRoot /Users/yourusername/Documents/ApacheVhosts/phptest
    <Directory /Users/yourusername/Documents/ApacheVhosts/phptest>
        AllowOverride All

        Require all granted

    </Directory>

</VirtualHost>



// add that vhost to your hosts file
sudo nano /etc/hosts
127.0.0.1	phptest.local
