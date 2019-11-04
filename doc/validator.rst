.. _topics-validator:

=========================
The Validator Component
=========================

*The Validator component provides tools to validate values following the JSR-303 Bean Validation specification.*


Installation
=============

.. note::
     If you install this component outside of a Symfony application, you must require the vendor/autoload.php file in your code to enable the class autoloading mechanism provided by Composer. 
	 
Type::
	  composer require symfony/validator


	 
Usage
========

The Validator component behavior is based on two concepts:

	* Constraints, which define the rules to be validated;
	* Validators, which are the classes that contain the actual validation logic.
	
The following example shows how to validate that a string is at least 10 characters long::

	use Symfony\Component\Validator\Constraints\Length;
	use Symfony\Component\Validator\Constraints\NotBlank;
	use Symfony\Component\Validator\Validation;

	$validator = Validation::createValidator();
	$violations = $validator->validate('Bernhard', [
		new Length(['min' => 10]),
		new NotBlank(),
	]);

	if (0 !== count($violations)) {
		// there are errors, now you can show them
		foreach ($violations as $violation) {
			echo $violation->getMessage().'<br>';
		}
	}
	
The validate() method returns the list of violations as an object that implements ConstraintViolationListInterface. If you have lots of validation errors, you can filter them by error code::

	use Symfony\Bridge\Doctrine\Validator\Constraints\UniqueEntity;

	$violations = $validator->validate(...);
	if (0 !== count($violations->findByCodes(UniqueEntity::NOT_UNIQUE_ERROR))) {
		// handle this specific error (display some message, send an email, etc.)
	}
	
	
Retrieving a Validator Instance
=================================

The Validator class is the main access point of the Validator component. To create a new instance of this class, it's recommended to use the Validation class::

	use Symfony\Component\Validator\Validation;

	$validator = Validation::createValidator();
	
This $validator object can validate simple variables such as strings, numbers and arrays, but it can't validate objects. To do so, configure the Validator class as explained in the next sections.


