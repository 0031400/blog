---
title: "php webdav"
description: "A simple php webdav server using sabre"
date: 2025-05-07T19:37:35+08:00
image: "image.png"
math: false
draft: false
---
### summary
php is a popular language for website.Many hosting provider provides cheap php server because their cost is also cheap.We can use these php server as a storage server.What protocol we can use?I find webdav is the most easy.And there is ready-made library for us to use.That is [https://github.com/sabre-io/dav](https://github.com/sabre-io/dav) Now I tell you how to transform a php server to a storage server with the library.
### docs
[https://sabre.io/dav/install](https://sabre.io/dav/install) is the official webdav library docs.We can see how to set up the webdav.
### donwload the library
Mkdir and cd the folder where we want to set up our server or the local folder in our computer.  
```bash
mkdir -p /var/www/webdav
cd /var/www/webdav
```
Download the library
```bash
composer require sabre/dav
```
### edit the server file
Write a file name `webdav.php` in the folder.Write the texts below to the file.
```php
<?php
use Sabre\DAV;
require 'vendor/autoload.php';

// This is where the webdav file folder is.
$rootDirectory = new DAV\FS\Directory('public');

// Set up the server
$server = new DAV\Server($rootDirectory);

// This is the url of the php file
$server->setBaseUri('/webdav.php');

// This is a lock plugin.It is optional.
// $lockBackend = new DAV\Locks\Backend\File('data/locks');
// $lockPlugin = new DAV\Locks\Plugin($lockBackend);
// $server->addPlugin($lockPlugin);

// This is a web control panel plugin.With it,we can get,send,list the files.It is optional.
// $server->addPlugin(new DAV\Browser\Plugin());

// This is for the auth.I think it is important.
// use Sabre\DAV\Auth;
// $authBackend = new Auth\Backend\File('htdigest');
// $authBackend->setRealm('SabreDAV');
// $authPlugin = new Auth\Plugin($authBackend);

// Run the server.
$server->exec();
```
### config the auth
The above code has a basic auth.We need to write our auth setting in a file like `htdigest`.We will use use a realm like `SabreDAV`  
Run the command to get the password hash. `php -r "echo md5('username:SabreDAV:password');"`The `username` is your username.The `password` is your passwrod.The `SabreDAV` is your realm.You will get a string like this.`5790c3784a79a018d1186528df520e11`  
You need to write the text like below to your htdigest file.
`username:SabreDAV:5790c3784a79a018d1186528df520e11`  
Remove the annatation in the file above and put the all files to your server.
### the zip I build
I build the above file and add the basic auth.The username is usernmae.The password is the password.  
The download link is [webdav.zip](webdav.zip) 
You can directly use it after change the username and password.
### config for alist
|name|value|
|-|-|
|Driver|WebDav|
|Mount Path|/webdav|
|Address|http://127.0.0.1:8080/webdav.php|
|Username|username|
|Password|password|
|Root folder path|/|