.. Dpi documentation master file, created by
   sphinx-quickstart on Thu Oct 31 07:40:25 2019.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

==============================
Symfony documentation
==============================

Symfony is a PHP `web application framework`_ and a set of reusable PHP components/libraries. It was originally conceived by the interactive agency SensioLabs for the development of web sites for its own customers. It was published as `free software`_ on October 18, 2005 and released under the MIT license.

Supported by SensioLabs - but also by a large community - Symfony provides many resources: plentiful documentation, community support (mailing lists, IRC, etc.), professional support (consulting, training, etc.), and so on.

Thousands of web sites and applications rely on the Symfony framework and the `Symfony components`_ as the foundation of their web services. And most of the leading PHP projects, such as Drupal, phpBB, Laravel and eZ Publish use Symfony components to build their applications.



.. _web application framework: https://en.wikipedia.org/wiki/Web_framework
.. _free software: https://en.wikipedia.org/wiki/Free_software
.. _free software: https://symfony.com/components

Goal
============

Symfony aims to speed up the creation and maintenance of web applications and to replace repetitive coding tasks. It's also aimed at building robust applications in an enterprise context, and aims to give developers full control over the configuration: from the directory structure to the foreign libraries, almost everything can be customized. To match enterprise development guidelines, Symfony is bundled with additional tools to help developers test, debug and document projects.

Symfony has a low performance overhead used with a bytecode cache.

First steps
===========

.. toctree::
   :caption: First steps
   :hidden:

   intro/overview
   intro/install
   intro/tutorial
   intro/examples

:doc:`intro/overview`
    Understand what you need before install Symfony

:doc:`intro/install`
    Get Symfony installed on your computer.

:doc:`intro/tutorial`
    The technological benefits of Symfony in 6 easy lessons.

:doc:`intro/examples`
    Learn more.
	
.. _section-basics:	

Create your First Page in Symfony
==================================
Creating a new page - whether it's an HTML page or a JSON endpoint - is a two-step process:

1. Create a route: A route is the URL (e.g. /about) to your page and points to a controller;
2. Create a controller: A controller is the PHP function you write that builds the page. You take the incoming request information and use it to create a Symfony Response object, which can hold HTML content, a JSON string or even a binary file like an image or PDF.


.. toctree::
   :caption: Create your First Page in Symfony
   :hidden:
   
   
	topics/routecontroller

:doc:`topics/routecontroller`
    Learn how to create a page.

	
	

Components
===========

.. toctree::
   :caption: Find different components for Symfony
   :hidden:
   
 
   topics/asset
   topics/cache
   topics/finder
   topics/validator

:doc:`topics/asset`
    Learn more about component ASSET

:doc:`topics/cache`
    Learn more about component CACHE

:doc:`topics/finder`
    Learn more about component FINDER

:doc:`topics/validator`
    Learn more about component VALIDATOR





.. image
.. toctree::
   :maxdepth: 2
   :caption: Contents:

