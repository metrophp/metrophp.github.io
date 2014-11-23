---
layout: page
title: "Quick Start"
excerpt: "Starting out with Metro PHP"
categories: docs
tags: [docs, intro, tutorial]
image:
  feature: so-simple-sample-image-2.jpg
  credit: WeGraphics
  creditlink: http://wegraphics.net/downloads/free-ultimate-blurred-background-pack/
comments: true
share: true
---


Your First Request
=====
Start by cloning the git repository _metroproject_.  
```bash
  git clone https://github.com/metrophp/metroproject.git mynewproj
  cd mynewproj
  rm -Rf .git
  git init .
```

Copy the etc/bootstrap.php.txt to etc/bootstrap.php
```bash
cp etc/bootstrap.php.txt etc/bootstrap.php
```
Initialize dependencies with composer
```bash
composer install
```

Now let's add a simple hello world file so you can see how the configuration works. Edit the etc/bootstrap.php file so it looks like this:
```php
// etc/bootsrap.php
_iCanHandle('output',  'example/helloworld.php');
```

Create a directory in src called "example" and create the helloworld.php file
```bash
mkdir src/example
```

```php
<?php

class Example_Helloworld {

	public function output($request, $response) {
		echo "Hello World. <br/>\n";
	}
}
```
Load your site and check your results.

Class Name Mapping
===
Notice how the classname directly maps to the folder/file structure on the disk. example/helloworld.php directly translates to a file Example_Helloworld. Nofw prefers the "has-a" over the "is-a" paradigm. Consequently, you won't have modules or libraries that need nested folders. Having one way of mapping saves a lot of processing overhead as well. The trade-off is that sometimes your class names might appear ugly, but the upside is that you focus on how those clases connect to produce your application.

Don't worry, you can also connect any classname with or without namespaces and with or without autoloading into the lifecycles with _\_iCanHandle()_
