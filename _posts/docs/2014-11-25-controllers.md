---
layout: doc
title: "Controllers & Actions"
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

Controllers
=======
A controller in Metro is just a POPO (Plain ol' PHP object).  How it connects with the system is that it defines a hook
to catch the incoming request.  If you don't define this hook - or the object - nothing bad happens.

{% highlight PHP startinline %}

class Example_Controller {

    public function mainAction($request, $response) {
    }
}
{% endhighlight %}

Now, this class is available at *http://localhost/mynewproj/index.php/example/controller*.  You might wonder why the
dictates what URL the controller is available at.  But that's the wrong way to think about it.  When you're designing
a solution, you're going to think about the URL first, then you'll naturally know where to go to make the controller.
Keeping the name mapping so stiff allows for small router logic.  And if you want performance, you need to optimize
things that happen *every* request, like routers.

That's it
----

Seriously.  Add some items to the *$response* with 
{% highlight PHP startinline %}
$response->addTo('main', 'somthing');  //main corresponds to Metrofw_Template::parseSection('main') in the template
{% endhighlight %}

But how do I get a DB or User object?
----
Oh you want your controller to have magic getters?  Well, here's how you can get anything that has been *_dedef()*ed 
previously.

{% highlight PHP startinline %}

class Example_Controller {
    public function mainAction($request, $response, $user, $db) {
    }
}
{% endhighlight %}

That's it.  Yes, it's a little bit of magic, but much easier to mock and test than dozes of magic getters.
