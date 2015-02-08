# Directory Structure

## Table of Contents
1. [Introduction](#introduction)
2. [app](#app)
3. [bootstrap](#bootstrap)
4. [configs](#configs)
5. [public](#public)
6. [resources](#resources)
7. [tests](#tests)
8. [tmp](#tmp)

<a id="introduction"></a>
## Introduction
RDev's directory structure was inspired by the best of ideas from frameworks like Laravel and Aura.  The directories logically separate files based on their purpose.  However, you are not chained to this structure.  If you do decide to customize it, make sure you update the PSR-4 settings "composer.json" as well as the relevant paths in "configs/paths.php".  

<a id="app"></a>
## app
This is where your project's core code goes.  If it's a PHP class, it belongs in here.  Bootstrappers are under the "bootstrappers" subdirectory, console commands under "console/commands", and HTTP controllers under "http/controllers".

<a id="bootstrap"></a>
## bootstrap
This contains the code that actually boots up your application.  It's probably best not to touch the contents unless you are sure you know what you are doing.

<a href="config"></a>
## configs
All configuration files for your application should go here.  Console-specific configs are in the "console" subdirectory, and web-specific configs are in the "http" subdirectory.  The "environment" subdirectory holds .env.*.php files to setup your server with environment variables.

<a id="public"></a>
## public
Like the name implies, this is the directory that is publicly-accessible.  Assets like CSS, JavaScript, and images should go under the "assets" subdirectory.

> **Note:** ".htaccess" and "index.php" are essential to routing HTTP requests.  Only edit these if you know what you are doing.

<a id="resources"></a>
## resources
This is where your views and other non-publicly-accessible files go.  For example, SCSS files and un-minified JavaScript files belong here.
 
<a id="tests"></a>
## tests
Put your unit tests in this directory.

<a id="tmp"></a>
## tmp
Any files that are generated by your application, such as compiled views, are stored here.  This is different than the "resources" directory in that files here are meant to be read and written to by the application, not a developer.