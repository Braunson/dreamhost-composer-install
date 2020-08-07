Installing Composer on a shared Dreamhost Server - 2020 Update
==========================
Some people have had a hell of a time installing Composer on a shared DH account, well here's how I did it. I'm going to assume you know what a shell user is and how to use basic terminal.

-----------------------

## Supported PHP Version

DreamHost ocassionally introduces new PHP versions and removes older ones. To keep updated on what is supported, please check [this link](https://help.dreamhost.com/hc/en-us/articles/215082337-What-versions-of-PHP-are-available-at-DreamHost-).


Requirements
-------------------------
1. A shared Dreamhost account
2. Make sure your user is SFTP (shell access)
3. I suggest updating your domain that your installing Composer on to run PHP 7.4.x w/ FastCGI
4. Wait 5-10 minutes for everything to move to 7.4 (if your not there already)


Step 1: Running PHP 7.4 in CLI (Command Line Interface)
-------------------------

1. Login via SSH (if you are not logged in already)
2. Navigate to ~/ (`$ cd ~/`)
3. Create or edit the existing file (`$ nano .bash_profile`)
4. On a new line copy and paste this code
  > export PATH=/usr/local/php74/bin:$PATH

5. Save the file and exit (CTRL+O then CTRL+X)
6. You may need to logout and log back in
7. **If you've decided to run a different supported PHP version, make sure to update the export string in the path**


Step 2: Enable the Phar Extension
-------------------------

1. Login via Putty or whatever flavor you like
2. Navigate to `$ cd ~/`
3. Create the folder `$ mkdir -p ~/.php/7.4`
4. Create the file `$ nano ~/.php/7.4/phprc`
5. Pop these jewels into the file on seperate lines

 > extension = phar.so<br /> # This line may or may not be needed.
 > suhosin.executor.include.whitelist = phar<br />
 > pcre.jit=0

  **NOTE:** The first line may or may not be needed, in my case PHP 7.4 on Dreamhost already had the Phar extension enabled.

6. Save the file and exit (CTRL+O then CTRL+X)


Step 3: Installing Composer
-------------------------
1. After you've completed the above steps, log out of putty and then log back in, run the command `$ php -v` or of that does not work for
  you run `$ php --version` and it should say that it's running PHP 7.4 CLI.

  It should look something like this:
 > PHP 7.4.3 (cli) (built: Feb 21 2020 00:45:59) ( NTS )<br />
 > Copyright (c) The PHP Group<br />
 > Zend Engine v3.4.0, Copyright (c) Zend Technologies<br />
 >  with Zend OPcache v7.4.3, Copyright (c), by Zend Technologies

  If not, then you've messed up somewhere along the line.

2. Now you can run the following command in the directory you want to use Composer in (go to the directory first them run this command)
 > curl -sS https://getcomposer.org/installer | php
 > mv ~/.php/composer/composer.phar ~/.php/composer/composer

3. You should see some installation success messages; this is good!

  If you see the following error:

  > \#!/usr/bin/env php
  > Some settings on your machine make Composer unable to work properly.
  > Make sure that you fix the issues listed below and run this script again:
  >
  > The phar extension is missing.
  > Install it or recompile php without --disable-phar

  This means you're on a DH server that has a different set of expectations for phrc. To fix it:

  ````
    $ cd ~/.php
    $ mkdir 7.4
    $ mv phprc 7.4/phprc
  ````

  (or, if you're using PHP 7.4, that would be `$ mkdir 7.4` and `$ mv phprc 7.4/phprc`)
4. If you don't see any errors, it should have been installed correctly. Next add this handy line so we can access the Composer command globally:
  Run `$ nano ~/.bash_profile` and add this on a new line:
  > export PATH=/home/your_username/.php/composer:$PATH

5. Try running `composer`, you should see a list of green and grey items, you've installed composer
6. I'll assume from here on our you can read the Composer documentation and usage :)


Step 4: Success
-------------------------
You should not be able to run `composer install` in your project folder.
