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
{% highlight bash %}
git clone https://github.com/metrophp/metroproject.git mynewproj
cd mynewproj
rm -Rf .git
git init .
{% endhighlight %}

Copy the etc/bootstrap.php.txt to etc/bootstrap.php
{% highlight bash %}
cp etc/bootstrap.php.txt etc/bootstrap.php
{% endhighlight %}

Initialize dependencies with composer
{% highlight bash %}
composer install
{% endhighlight %}

Don't Panic, I Can Handle It
======

Now let's add a simple hello world file so you can see how the configuration works. Edit the etc/bootstrap.php file so it looks like this:
{% highlight php %}
// etc/bootsrap.php
_iCanHandle('output',  'example/helloworld.php');
{% endhighlight %}

The *\_iCanHandle* function tells the kernel that the file **example/helloworld.php** wants to participate in the **ouput** lifecycle

Create a directory in src called "example" and create the helloworld.php file
{% highlight bash %}
mkdir src/example
{% endhighlight %}

{% highlight php %}
<?php
class Example_Helloworld {

	public function output($request, $response) {
		echo "Hello World. <br/>\n";
	}
}
{% endhighlight %}

Load your site and check your results.

Notice how the example class doesn't need to extend any parent class.  The Metro PHP kernel uses a the name of the lifecycle handled by the component as a method name to act as the entry point of that component into the framework.

Class Name Mapping
===
Notice how the classname directly maps to the folder/file structure on the disk. example/helloworld.php directly translates to a file Example_Helloworld. Nofw prefers the "has-a" over the "is-a" paradigm. Consequently, you won't have modules or libraries that need nested folders. Having one way of mapping saves a lot of processing overhead as well. The trade-off is that sometimes your class names might appear ugly, but the upside is that you focus on how those clases connect to produce your application.

Don't worry, you can also connect any classname with or without namespaces and with or without autoloading into the lifecycles with *\_iCanHandle()*
