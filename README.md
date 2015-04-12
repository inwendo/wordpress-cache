## Overview

This is a WordPress object cache backend that supports the MemCachier caching
service. It requires [Memcached ver. 2.2.0 PECL
package](http://pecl.php.net/package/memcached) or greater for SASL support.

It is a fork of
[tollmanz](https://github.com/tollmanz/wordpress-pecl-memcached-object-cache)
object cache, adding SASL and binary protocol support for cloud based caches.

## Installation

1. Make sure you have [libmemcached](http://libmemcached.org/libMemcached.html)
   installed, built with SASL. See the [Memcached
   Requirements](http://il1.php.net/manual/en/memcached.requirements.php). 

2. Install the [Memcached ver. 2.2.0 PECL
   package](http://il1.php.net/manual/en/memcached.installation.php).

3. Define the Memcached servers and SASL credentials in your wp-config.php, as
   follows:

		global $memcached_servers;
		$memcached_servers = array( array( 'host', port ) );

		global $memcached_username;
		$memcached_username = 'sasl_username';

		global $memcached_password;
		$memcached_password = 'sasl_password';

4. Alternatively, set the environment variables, `MEMCACHE_SERVERS`,
   `MEMCACHE_USERNAME` and `MEMCACHE_PASSWORD`. The expected format for
   `MEMCACHE_SERVERS` is `'server1:port1,server2:port2,server3:port3'`.

**Note:** If running on Heroku or AppFog, just install the MemCachier add-on
and your conifguration environment variables will be set.  

4. Move object-cache.php to wp-content/object-cache.php

5. To test the WordPress object cache setup, add the following code as an MU
   plugin:

	```php
	<?php
	$key   = 'dummy';
	$value = '100';

	$dummy_value = wp_cache_get( $key );

	if ( $value !== $dummy_value ) {
		echo "The dummy value is not in cache. Adding the value now.";
		wp_cache_set( $key, $value );
	} else {
		echo "Value is " . $dummy_value . ". The WordPress Memcached Backend is working!";
	}
	```

  After adding the code, reload your WordPress site twice. On the second load,
  you should see a success message printed at the top of the page. Remove the
  MU plugin after you've verified the setup.

## Credits

* [wordpress-memcached-backend](https://github.com/tollmanz/wordpress-memcached-backend)
* [RedisLabs SASL support](https://github.com/RedisLabs/wordpress-memcached-backend)

