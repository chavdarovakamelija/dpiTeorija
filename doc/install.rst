.. _intro-install:

==================
Installing and Configuring Sympfony
==================
The goal is to get you up and running with a working application built on top of Symfony.
In order to simplify the process of creating new applications, Symfony provides an installer application.

Installing the Symfony Installer
=================

Using the **Symfony Installer** is the only recommended way to create new Symfony applications. This installer is a PHP application that has to be installed in your system only once and then it can create any number of Symfony applications.

.. note::

   The installer requires PHP 5.4 or higher. If you still use the legacy PHP 5.3 version, you cannot use the Symfony Installer. Read the Creating Symfony Applications without the Installer section to learn how to proceed.

Depending on your operating system, the installer must be installed in different ways.


Linux and Mac OS X Systems
^^^^^^^^^^^^^^^^^^^^^^^^^^^
Open your command console and execute the following commands::

    $ sudo curl -LsS https://symfony.com/installer -o /usr/local/bin/symfony
	$ sudo chmod a+x /usr/local/bin/symfony

This will create a global **symfony** command in your system.


WindowsX Systems
^^^^^^^^^^^^^^^^^^^^^^^^^^^
Open your command console and execute the following commands::

    c:\> php -r "readfile('https://symfony.com/installer');" > symfony

Then, move the downloaded **symfony** file to your project's directory and execute it as follows::
   
    c:\> move symfony c:\projects
    c:\projects\> php symfony



Creating the Symfony Application
-----------------------------------------

Once the Symfony Installer is available, create your first Symfony application with the **new** command::

    # Linux, Mac OS X
	$ symfony new my_project_name
	# Windows
	c:\> cd projects/
	c:\projects\> php symfony new my_project_name

This command creates a new directory called **my_project_name/** that contains a fresh new project based on the most recent stable Symfony version available. In addition, the installer checks if your system meets the technical requirements to execute Symfony applications. If not, you'll see the list of changes needed to meet those requirements.

.. note::
    If the installer doesn't work for you or doesn't output anything, make sure that the PHP Phar extension2 is installed and enabled on your computer.

The installer also supports a special version called lts which installs the most recent Symfony LTS version available::

	$ symfony new my_project_name lts



Running the Symfony Application
-----------------------------------------

Symfony leverages the internal web server provided by PHP to run applications while developing them. Therefore, running a Symfony application is a matter of browsing the project directory and executing this command::

	$ cd my_project_name/
	$ php app/console server:run

Then, open your browser and access the **http://localhost:8000/** URL to see the Welcome Page of Symfony:

.. image:: ../images/Capture.png
    :width: 500px
    :align: center
    :height: 300px
    :alt: alternate text



Instead of the Welcome Page, you may see a blank page or an error page. This is caused by a directory permission misconfiguration. There are several possible solutions depending on your operating system.
All of them are explained in the Setting up Permissions section of this chapter.
PHP's internal web server is great for developing, but should **not** be used on production. Instead, use Apache or Nginx. See Configuring a Web Server.

When you are finished working on your Symfony application, you can stop the server by pressing Ctrl+C
from terminal.
