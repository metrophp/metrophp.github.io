DI Container
========
DI is handled by the [Metrodi's Container](https://github.com/metrophp/metrodi) class.
This DI container is unlike most others in that - when inpecting constructor arguments - it defaults to matching the parameter **name** before attempting to match the type hint.

{% highlight php %}
class MyAuditLog
    public function __construct($user, $log) {
}
{% endhighlight %}

Such a class, when created with the DI container, will be passed any objects you specify
as a "user" and a "log".

{% highlight php %}
_didef('user', 'my/log.php');  //Requires a class called My_Log in that file
_didef('log', 'Monolog\Logger');  //Can use any class name
{% endhighlight %}

Setting up and using the container
====
Metro DI can be used by creating an object or as a singleton with globally namespaced functions.
{% highlight php %}
    $cont = Metrodi_Container::getContainer();
    $cont->set('flaga', true);
    $cont->didef('logService', 'path/to/log.php', 'arg1', 'arg2', 'arg3');
    $log = $cont->make('logService');
{% endhighlight %}

With global functions it looks like this:
{% highlight php %}
    _set('flaga', true);
    _didef('logService', 'path/to/log.php', 'arg1', 'arg2', 'arg3');
    $log = _make('logService');
{% endhighlight %}

Defining Things
---
In Metro DI you name your dependencies with the **_didef** function.

{% highlight php %}
    _didef('shoppingCart', 'My\Cart\Cart');
    $cart = _make('shoppingCart');
    echo( get_class($cart) );  //  My\Cart\Cart
{% endhighlight %}

Constructor Injection
---
When another class wants a **$shoppingCart** as a constructor param, the DI container will
automatically inject the defined class if you make the object with the DI container.

{% highlight php %}
class CartController {
  public function __construct($shoppingCart) {
    echo get_class($shoppingCart); // My\Cart\Cart
  }
}
{% endhighlight %}

Passing Parameters to Things
---
You can pass parameters both when defining an object and when making an object.

{% highlight php %}
	namespace org\my\cart\service\abstract\concrete\interface;
	class Cart {
		public function __construct($idUser, $listItems, $timestamp=NULL) {
			$this->idUser    = $idUser;
			$this->listItems = $listItems;
			$this->timestamp = $timestamp;
		}
	}

	_didef('shoppingCartService', '\org\my\cart\service\abstract\concrete\interface\Cart', 'A', 'B');
	$cart1 = _make('shoppingCartService');
	echo $cart1->idUser;     // 'A'
	echo $cart1->listItems;  // 'B'
	echo $cart1->timestamp;  // null

	$cart2 = _make('shoppingCartService', 'C', 'D', time());
	echo $cart2->idUser;     // 'C'
	echo $cart2->listItems;  // 'D'
	echo $cart2->timestamp;  // 1234567890  (YMMV)
{% endhighlight %}

Singletons
----------
Singletons and new objects can both be access with \_make() but it depends on how you use \_make() with \_didef().

{% highlight php %}
   _didef('singletonService', '\ns\locator\class', 'arg1', 'arg2');

   //later
   $ss1 = _make('singletonService');

   //same reference
   $ss2 = _make('singletonService');
{% endhighlight %}

If your object is not inherently a service and needs new constructor params everytime you make it, you can pass
constructor params for every \_make() call.  Each unique combinations of parameters will be hashed and the resulting
object will be cached and return on subsequent calls to \_make()

{% highlight php %}
   _didef('user', '\ns\locator\class');

   //later
   $u1 = _make('user', 100);

   //new object
   $u2 = _make('user', 200);
{% endhighlight %}

You can combine both methods by supplying parameters to the \_didef() which will be combined and used as defaults to
subsequent \_make() calls

{% highlight php %}
   _didef('log', '\ns\locator\class', '/tmp/out.log');

   //later
   $mainLog = _make('log');
   
   $specialLog = _make('log', '/var/log/special.log');
{% endhighlight %}


Undefined Things
------
An undefined thing will return a prototype object that logs all method calls used against it via the magic __call.

{% highlight php %}
    $cart = _make('shoppingCartService');
    echo( get_class($cart) );  //  Metrodi_Proto

    _didef('shoppingCartService', 'path/to/my/cart.php');
    $cart = _make('shoppingCartService');
    echo( get_class($cart) );  //  Path_To_My_Cart
{% endhighlight %}

A note on file names
====
Why does _didef accept just a file and have old PHP naming conventions (i.e. NS_Class)?  Because they're faster.  Autoloading can add consitently 10-20% overhead on your project and shouldn't be used for classes that are required every hit if you want performance.

Autoloading can be extremely usefull when you are building your libraries and applications, but a framework should be optimized - or at least optimize-*able* - when dealing with common libraries that are needed every request.

