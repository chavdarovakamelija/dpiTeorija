.. _topics-routecontroller:

========================================
Creating a Page: Route and Controller
========================================

Suppose you want to create a page **- /lucky/number **- that generates a lucky (well, random) number and prints it. To do that, create a "Controller" class and a "controller" method inside of it::


	<?php
	// src/Controller/LuckyController.php
	namespace App\Controller;

	use Symfony\Component\HttpFoundation\Response;

	class LuckyController
	{
		public function number()
		{
			$number = random_int(0, 100);

			return new Response(
				'<html><body>Lucky number: '.$number.'</body></html>'
			);
		}
	}

Now you need to associate this controller function with a public URL (e.g. /lucky/number) so that the number() method is executed when a user browses to it. This association is defined by creating a **route** in the config/routes.yaml file::

	# config/routes.yaml

	# the "app_lucky_number" route name is not important yet
	app_lucky_number:
		path: /lucky/number
		controller: App\Controller\LuckyController::number

That's it! If you are using Symfony web server, try it out by going to:

That's it! If you are using Symfony web server, try it out by going to:

.. attribute:: http://localhost:8000/lucky/number

If you see a lucky number being printed back to you, congratulations! But before you run off to play the lottery, check out how this works. Remember the two steps to creating a page?

	1. *Create a route:* In config/routes.yaml, the route defines the URL to your
page (path) and what controller to call. You'll learn more about `routing`_ in its own section, including how to make variable URLs;

	2. *Create a controller:* This is a function where you build the page and ultimately return a Response object. You'll learn more about `controllers`_ in their own section, including how to return JSON responses.


.. _routing: https://symfony.com/doc/current/routing.html
.. _controllers: https://symfony.com/doc/current/controller.html


Annotation Routes
==========================

Instead of defining your route in YAML, Symfony also allows you to use annotation routes. To do this, install the annotations package::

	 composer require annotations

You can now add your route directly *above* the controller::
	// src/Controller/LuckyController.php

	// ...
	+ use Symfony\Component\Routing\Annotation\Route;

	class LuckyController
	{
	+     /**
	+      * @Route("/lucky/number")
	+      */
		public function number()
		{
			// this looks exactly the same
		}
	}

That's it! The page - **http://localhost:8000/lucky/number** will work exactly like before! Annotations are the recommended way to configure routes.



The bin/console Command
=========================
Your project already has a powerful debugging tool inside: the bin/console command. Try running it::

	 php bin/console

You should see a list of commands that can give you debugging information, help generate code, generate database migrations and a lot more. As you install more packages, you'll see more commands.

To get a list of *all* of the routes in your system, use the debug:router command::

	 php bin/console debug:router


You should see your app_lucky_number route at the very top:
	
+------------------+------------+---------------+
|Name              | Host       | Path          |
+==================+============+===============+
| app_lucky_number | ANY        | /lucky/number |
+------------------+------------+---------------+


You will also see debugging routes below app_lucky_number -- more on the debugging routes in the next section.

You'll learn about many more commands as you continue!


Rendering a Template
=====================

If you're returning HTML from your controller, you'll probably want to render a template. Fortunately, Symfony comes with Twig: a templating language that's easy, powerful and actually quite fun.

Make sure that LuckyController extends Symfony's base AbstractController class::

	// src/Controller/LuckyController.php

	// ...
	+ use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;

	- class LuckyController
	+ class LuckyController extends AbstractController
	{
		// ...
	}
	
Now, use the handy render() function to render a template. Pass it a number variable so you can use it in Twig::

	// src/Controller/LuckyController.php
	namespace App\Controller;

	// ...
	class LuckyController extends AbstractController
	{
		/**
		* @Route("/lucky/number")
		*/
		public function number()
		{
			$number = random_int(0, 100);

			return $this->render('lucky/number.html.twig', [
				'number' => $number,
			]);
		}
	}
	
Template files live in the templates/ directory, which was created for you automatically when you installed Twig. Create a new templates/lucky directory with a new number.html.twig file inside::

	{# templates/lucky/number.html.twig #}
	<h1>Your lucky number is {{ number }}</h1>

The {{ number }} syntax is used to print variables in Twig. Refresh your browser to get your new lucky number!

.. attribute:: http://localhost:8000/lucky/number

Now you may wonder where the Web Debug Toolbar has gone: that's because there is no </body> tag in the current template. You can add the body element yourself, or extend base.html.twig, which contains all default HTML elements.

In the *templates* article, you'll learn all about Twig: how to loop, render other templates and leverage its powerful layout inheritance system.


Checking out the Project Structure
===================================

Great news! You've already worked inside the most important directories in your project:

 .. attribute:: config/
 
Contains... configuration!. You will configure routes, services and packages.

 .. attribute:: src/
 
All your PHP code lives here.

 .. attribute:: templates/
 
All your Twig templates live here.

Most of the time, you'll be working in **src/**, **templates/** or **config/**. As you keep reading, you'll learn what can be done inside each of these.

So what about the other directories in the project?

 .. attribute:: bin/
 
The famous bin/console file lives here (and other, less important executable files).

 .. attribute:: var/
 
This is where automatically-created files are stored, like cache files (var/cache/) and logs (var/log/).

 .. attribute:: vendor/
 
Third-party (i.e. "vendor") libraries live here! These are downloaded via the Composer package manager.

 .. attribute:: public/
 
This is the document root for your project: you put any publicly accessible files here.
And when you install new packages, new directories will be created automatically when needed.