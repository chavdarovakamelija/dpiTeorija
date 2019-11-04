.. _topics-finder:

=====================
The Finder Component
=====================

*The Finder component finds files and directories based on different criteria (name, file size, modification time, etc.) via an intuitive fluent interface.*


Installation
=============

.. note::
     If you install this component outside of a Symfony application, you must require the vendor/autoload.php file in your code to enable the class autoloading mechanism provided by Composer.
	 
Type::
	composer require symfony/finder


	 
Usage
========

The Finder class finds files and/or directories::

	use Symfony\Component\Finder\Finder;

	$finder = new Finder();
	// find all files in the current directory
	$finder->files()->in(__DIR__);

	// check if there are any search results
	if ($finder->hasResults()) {
		// ...
	}

	foreach ($finder as $file) {
		$absoluteFilePath = $file->getRealPath();
		$fileNameWithExtension = $file->getRelativePathname();

		// ...
	}

	
The $file variable is an instance of SplFileInfo which extends PHP's own SplFileInfo to provide methods to work with relative paths.

Searching for Files and Directories
========================================
The component provides lots of methods to define the search criteria. They all can be chained because they implement a fluent interface.


Location
-------------------

The location is the only mandatory criteria. It tells the finder which directory to use for the search::

	$finder->in(__DIR__);

Search in several locations by chaining calls to in():

	// search inside *both* directories
	$finder->in([__DIR__, '/elsewhere']);

	// same as above
	$finder->in(__DIR__)->in('/elsewhere');
	
Use * as a wildcard character to search in the directories matching a pattern (each pattern has to resolve to at least one directory path)::

	$finder->in('src/Symfony/*/*/Resources');

Exclude directories from matching with the exclude() method::

	// directories passed as argument must be relative to the ones defined with the in() method
	$finder->in(__DIR__)->exclude('ruby');
	
It's also possible to ignore directories that you don't have permission to read::

	$finder->ignoreUnreadableDirs()->in(__DIR__);

As the Finder uses PHP iterators, you can pass any URL with a supported PHP wrapper for URL-style protocols (ftp://, zlib://, etc.)::

	// always add a trailing slash when looking for in the FTP root dir
	$finder->in('ftp://example.com/');

	// you can also look for in a FTP directory
	$finder->in('ftp://example.com/pub/');
	
	
	
Reading Contents of Returned Files
-------------------------------------

The contents of returned files can be read with getContents()::

	use Symfony\Component\Finder\Finder;

	$finder = new Finder();
	$finder->files()->in(__DIR__);

	foreach ($finder as $file) {
		$contents = $file->getContents();

		// ...
	}
	
	
Sorting Results
-------------------------------------

Sort the results by name or by type (directories first, then files)::

	$finder->sortByName();

	$finder->sortByType();

