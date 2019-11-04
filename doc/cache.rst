.. _topics-cache:

=====================
The Cache Component
=====================

*The Cache component provides features covering simple to advanced caching needs. It natively implements PSR-6 and the Cache Contracts for greatest interoperability. It is designed for performance and resiliency, ships with ready to use adapters for the most common caching backends. It enables tag-based invalidation and cache stampede protection via locking and early expiration.*


Installation
=============

.. note::
     The component also contains adapters to convert between PSR-6, PSR-16 and Doctrine caches. 
Type::
	 composer require symfony/cache

	 
	

Cache Contracts
-------------------

This component includes two different approaches to caching:

PSR-6 Caching:
	A generic cache system, which involves cache "pools" and cache "items".
Cache Contracts:
	A simpler yet more powerful way to cache values based on recomputation callbacks.

	
All adapters support the Cache Contracts. They contain only two methods: get() and delete(). There's no set() method because the get() method both gets and sets the cache values.

The first thing you need is to instantiate a cache adapter. The FilesystemAdapter is used in this example::

	use Symfony\Component\Cache\Adapter\FilesystemAdapter;

	$cache = new FilesystemAdapter();
	
	
.. note::
     :Using the Cache Contracts approach is recommended: it requires less code boilerplate and provides cache stampede protection by default. ::
	 
Now you can retrieve and delete cached data using this object. The first argument of the get() method is a key, an arbitrary string that you associate to the cached value so you can retrieve it later. The second argument is a PHP callable which is executed when the key is not found in the cache to generate and return the value::

	use Symfony\Contracts\Cache\ItemInterface;

	// The callable will only be executed on a cache miss.
	$value = $cache->get('my_cache_key', function (ItemInterface $item) {
		$item->expiresAfter(3600);

		// ... do some HTTP request or heavy computations
		$computedValue = 'foobar';

		return $computedValue;
	});

	echo $value; // 'foobar'

	// ... and to remove the cache key
	$cache->delete('my_cache_key');
	
	
The Cache Contracts also comes with built in Stampede prevention. This will remove CPU spikes at the moments when the cache is cold. If an example application spends 5 seconds to compute data that is cached for 1 hour and this data is accessed 10 times every second, this means that you mostly have cache hits and everything is fine. But after 1 hour, we get 10 new requests to a cold cache. So the data is computed again. The next second the same thing happens. So the data is computed about 50 times before the cache is warm again. This is where you need stampede prevention

The first solution is to use locking: only allow one PHP process (on a per-host basis) to compute a specific key at a time. Locking is built-in by default, so you don't need to do anything beyond leveraging the Cache Contracts.

The second solution is also built-in when using the Cache Contracts: instead of waiting for the full delay before expiring a value, recompute it ahead of its expiration date. The Probabilistic early expiration algorithm randomly fakes a cache miss for one user while others are still served the cached value. You can control its behavior with the third optional parameter of get(), which is a float value called "beta".

By default the beta is 1.0 and higher values mean earlier recompute. Set it to 0 to disable early recompute and set it to INF to force an immediate recompute::



	use Symfony\Contracts\Cache\ItemInterface;

	$beta = 1.0;
	$value = $cache->get('my_cache_key', function (ItemInterface $item) {
		$item->expiresAfter(3600);
		$item->tag(['tag_0', 'tag_1']);

		return '...';
	}, $beta);


Available Cache Adapters
-------------------------
The following cache adapters are available:

* APCu Cache Adapter
* Array Cache Adapter
* Chain Cache Adapter
* Doctrine Cache Adapter
* Filesystem Cache Adapter
* Redis Cache Adapter
