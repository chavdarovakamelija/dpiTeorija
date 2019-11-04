.. _topics-asset:

=====================
The Asset Component
=====================

*The Asset component manages URL generation and versioning of web assets such as CSS stylesheets, JavaScript files and image files.*


Installation
=============

.. note::
     If you install this component outside of a Symfony application, you must require the vendor/autoload.php file in your code to enable the class autoloading mechanism provided by Composer. 
Type::

	 composer require symfony/asset

	 
	 
Usage
======

Asset Packeges
-------------------

The Asset component manages assets through packages. A package groups all the assets which share the same properties: versioning strategy, base path, CDN hosts, etc. In the following basic example, a package is created to manage assets without any versioning::

	use Symfony\Component\Asset\Package;
	use Symfony\Component\Asset\VersionStrategy\EmptyVersionStrategy;

	$package = new Package(new EmptyVersionStrategy());

	// Absolute path
	echo $package->getUrl('/image.png');
	// result: /image.png

	// Relative path
	echo $package->getUrl('image.png');
	// result: image.png

Packages implement PackageInterface, which defines the following two methods:

`getVersion()`_
	Returns the asset version for an asset.
`getUrl()`_
	Returns an absolute or root-relative public path.
With a package, you can:

	1. `version the assets;`_
	2. set a `common base path`_ (e.g. /css) for the assets;
	3. `configure a CDN`_ for the assets
	
	
.. _getVersion(): https://github.com/symfony/symfony/blob/4.3/src/Symfony/Component/Asset/PackageInterface.php
.. _getUrl(): https://github.com/symfony/symfony/blob/4.3/src/Symfony/Component/Asset/PackageInterface.php
.. _version the assets: https://symfony.com/doc/current/components/asset.html#component-assets-versioning
.. _configure a CDN: https://symfony.com/doc/current/components/asset.html#component-assets-cdn



Grouped Assets
-------------------
Often, many assets live under a common path (e.g. /static/images). If that's your case, replace the default Package class with PathPackage to avoid repeating that path over and over again::

	use Symfony\Component\Asset\PathPackage;
	// ...

	$pathPackage = new PathPackage('/static/images', new StaticVersionStrategy('v1'));

	echo $pathPackage->getUrl('logo.png');
	// result: /static/images/logo.png?v1

	// Base path is ignored when using absolute paths
	echo $pathPackage->getUrl('/logo.png');
	// result: /logo.png?v1

