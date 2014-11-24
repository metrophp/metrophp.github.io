---
layout: doc
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

Initialize dependencies with composer
{% highlight bash %}
composer install
{% endhighlight %}

Don't Panic, I Can Handle It
======

Now let's add a simple hello world file so you can see how the configuration works. Edit the etc/bootstrap.php file and add the following to the bottom of the file:
{% highlight php startinline %}
// paste at bottom etc/bootsrap.php
_iCanHandle('hangup',  'example/helloworld.php');
{% endhighlight %}

The *\_iCanHandle* function tells the kernel that the file **example/helloworld.php** wants to participate in the **hangup** lifecycle

Create a directory in src called "example" and create the helloworld.php file
{% highlight bash %}
mkdir src/example
{% endhighlight %}

{% highlight php %}
<?php
class Example_Helloworld {

	public function hangup($request, $response) {
		global $metrofw_start;
                echo "Hello World. <br/>\n";
                printf (" Request took: %.2f ms" ,(microtime(true) - $metrofw_start)*1000);
	}
}
{% endhighlight %}

Load any page on your installation and check your results.  We should see "Hello World" and the processing time on every page.  (The hangup lifecycle is part of every page request).

Notice how the example class doesn't need to extend any parent class.  The Metro PHP kernel uses a the name of the lifecycle handled by the component as a method name to act as the entry point of that component into the framework.

Class Name Mapping
===
Notice how the classname directly maps to the folder/file structure on the disk. example/helloworld.php directly translates to a file Example_Helloworld. Nofw prefers the "has-a" over the "is-a" paradigm. Consequently, you won't have modules or libraries that need nested folders. Having one way of mapping saves a lot of processing overhead as well. The trade-off is that sometimes your class names might appear ugly, but the upside is that you focus on how those clases connect to produce your application.

Don't worry, you can also connect any classname with or without namespaces and with or without autoloading into the lifecycles with *\_iCanHandle()*

Adding a Controller
===========
Adding a controller is as simple as adding a Plain Ol' PHP Object.
{% highlight php %}
<?php
class Example_Main {

    public function mainAction($request, $response) {
        $response->addTo('main', 'Hello, ');
        $response->addTo('main', $request->cleanString('name'));
    }
}
{% endhighlight %}
Paste that into **src/example/main.php**.  When you load http://localhost/mynewproj/index.php/example?name=Metro you should see "Hello, Metro" printed (below the one on the default template).

URL Structure
----
The default URL mapping goes like this:

    /index.php/appName/modName/actionName
    /src/appName/modName.php :: actionNameAction()
 
where any missing parameter is defaulted to the word "main".  So, accessing http://localhost/mynewproj/index.php/example/foo/bar/name=baz/ would execute a function like

{% highlight php %}
<?php
class Example_Foo {

    public function barAction($request, $response) {
        $response->addTo('main', 'Hello, ');
        $response->addTo('main', $request->cleanString('name'));
    }
}
{% endhighlight %}


