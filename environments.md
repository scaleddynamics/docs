# Environments

## Table of Contents
1. [Introduction](#introduction)
2. [Hosts](#hosts)
  1. [Host Name](#host-name)
  2. [Host Regular Expression](#host-regular-expression)
3. [Environment Resolver](#environment-resolver)
4. [Environment Variables](#environment-variables)

<h2 id="introduction">Introduction</h2>
Sometimes, you might want to change the way your application behaves depending on whether or not it's running on a production, staging, testing, or development machine.  A common example is a database connection - each environment might have different server credentials.  By detecting the environment, you can load the appropriate data.

<h2 id="hosts">Hosts</h2>
Opulence uses host classes to help the application determine which environment it's running in.  Hosts implement `Opulence\Applications\Environments\Hosts\IHost`.

<h3 id="host-name">Host Name</h3>
If you're adding a rule for a single, specific host, use `HostName`:

```php
use Opulence\Applications\Environments\Hosts\HostName;

$host = new HostName("127.0.0.1");
```

<h3 id="host-regular-expression">Host Regular Expression</h3>
You can use a regular expression to match hosts by using `HostRegex`:

```php
use Opulence\Applications\Environments\Hosts\HostRegex;

$host = new HostRegex("^127\.0\.0\.\d+$");
```

> **Note:** You do not need to add regular expression delimiters.  The "#" character is used as the default delimiter.

<h2 id="environment-resolver">Environment Resolver</h2>
The environment resolver registers hosts with environments and attempts to find the correct host in the registry.  If no matching host is found, then `"production"` is returned.  `EnvironmentResolver::resolve()` returns the name of the current environment.  We can use this value to set the name of the environment:

```php
use Opulence\Applications\Environments\Environment;
use Opulence\Applications\Environments\Hosts\HostName;
use Opulence\Applications\Environments\Hosts\HostRegex;
use Opulence\Applications\Environments\Resolvers\EnvironmentResolver;

$resolver = new EnvironmentResolver();
$resolver->registerHost("production", new HostName("192.168.1.1"));
$resolver->registerHost("testing", new HostRegex("^127\.0\.0\.\d+$"));
$environmentName = $resolver->resolve(gethostname());
$environment = new Environment($environmentName);
```

<h2 id="environment-variables">Environment Variables</h2>
Variables that are specifically tied to the environment the application is running on are called **environment variables**.  Setting an environment variable using Opulence is as easy as `$environment->setVar("foo", "bar")`.  To get the environment variable, you can either call `$environment->getVar("foo")` or use PHP's native `getenv("foo")`.

To make configuring your environment variables as easy as possible, Opulence supports environment config files, whose names are of the format ".env.DESCRIPTION_OF_CONFIG.php".  They should exist in your "configs/environment" directory.  These files are automatically run before the application is booted up.  Let's take a look at an example:
 
##### .env.example.php
```php
$environment->setVar("DB_HOST", "localhost");
$environment->setVar("DB_USER", "myuser");
$environment->setVar("DB_PASSWORD", "mypassword");
$environment->setVar("DB_NAME", "public");
$environment->setVar("DB_PORT", 5432);
```

> **Note:** For performance reasons, .env.*.php files are only loaded on non-production servers.  It is strongly recommended that production servers are setup with hard-coded environment variables in their configs.  For security, it's strongly recommended that you do not version-control your environment variable configs.  Instead, each developer should be given a template of the environment config (eg .env.example.php), and should fill out the config with the appropriate values for their environment.

When you install Opulence, you'll see two environment config files:

1. `.env.example.php`
  * Values in here will not be used - it only serves as a template
  * This can be checked into version control
2. `.env.app.php`
  * Where your actual environment variables should be stored
  * This should not be checked into version control