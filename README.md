Installing Composer on a shared Dreamhost Server
==========================

Some people have had a hell of a time installing Composer on a shared DH account, well here's how I did it. 
I'm going to assume you know what a shell user is and how to use basic terminal

Requirements
-------------------------
1. A shared Dreamhost account
2. Make sure your user is sftp (shell access)
3. I suggest updating your domain that your installing composer on to run PHP 5.4.x w/ FastCGI
4. Wait 5-10 minutes for everything to move to 5.4 (if your not there already)


Step 1: Running PHP 5.4 (or 5.3) in CLI (Command Line Interface)
-------------------------

1. Login via Putty (if you are not logged in already)
2. Navigate to ~/
3. Create or edit the existing file (nano .bash_profile)
4. On a new line copy and paste this code
  > export PATH=/usr/local/php54/bin:$PATH

5. **If you've decided to run PHP 5.3 instead of PHP 5.4 change *php54* to *php53* in the path**

Step 2: Enable the Phar Extension
-------------------------

1. Login via Putty or whatever flavor you like
2. Navigate to ~/ (cd ~/)
3. Create the folder ~/.php (mkdir .php)
4. Create the file ~./.php/phprc (nano phprc)
5. Pop these jewels into the file on seperate lines
 > extension = phar.so<br />
 > suhosin.executor.include.whitelist = phar

6. Save the file and exit (CTRL+O then CTRL+X)


Step 3: Hurray!
-------------------------
1. After you've completed steps 1 and 2, log out of putty and then log back in, run the command **php -v** or of that does not work for
  you run **php --version** and it should say that it's running PHP 5.4 CLI.

  It should look something like this:
> PHP 5.4.11 (cli) (built: Feb 12 2013 19:02:05)<br />
> Copyright (c) 1997-2013 The PHP Group <br />
> Zend Engine v2.4.0, Copyright (c) 1998-2013 Zend Technologies

  If not, then you've messed up somewhere along the line.

2. Now you can run the following command in the directory you want to use Composer in (go to the directory first them run this command)
 > curl -s https://getcomposer.org/installer | php
3. You should see some installation success messages, this is good!
4. If you don't see any errors, it should have been installed correctly, now try running it
 > php composer.phar
5. By running the command above, you should see a list of green and grey items, you've installed composer
6. I'll assume from here on our you can read the composer documentation and usage :)


