Laravel Rollbar
===============

[![Build Status](http://img.shields.io/travis/jenssegers/laravel-rollbar.svg)](https://travis-ci.org/jenssegers/laravel-rollbar) [![Coverage Status](http://img.shields.io/coveralls/jenssegers/laravel-rollbar.svg)](https://coveralls.io/r/jenssegers/laravel-rollbar)

Rollbar error monitoring integration for Laravel projects. This library will add a listener to Laravel's logging component. All Rollbar messages will be pushed onto Laravel's queue system, so that they can be processed in the background without slowing down the application. Laravel's session data will also be sent to Rollbar.

![rollbar](https://d37gvrvc0wt4s1.cloudfront.net/static/img/features-dashboard1.png?ts=1361907905)

Installation
------------

Add the package to your `composer.json` and run `composer update`.

    {
        "require": {
            "jenssegers/rollbar": "*"
        }
    }

Add the service provider in `app/config/app.php`:

    'Jenssegers\Rollbar\RollbarServiceProvider',

Configuration
-------------

### Option 1: Services configuration file

This package supports configuration through the services configuration file located in `app/config/services.php`. All configuration variables will be directly passed to Rollbar:

    'rollbar' => array(
        'access_token' => 'your-rollbar-token',
    ),

### Option 2: The package configuration file

Publish the included configuration file:

    php artisan config:publish jenssegers/rollbar

And change your rollbar access token:

    'access_token' => 'your-rollbar-token',

### Attention!

Because this library uses the queue system, make sure your `config/queue.php` file is configured correctly. If you do not wish to process the jobs in the background, you can set the queue driver to 'sync':

    'default' => 'sync',

Usage
-----

This library adds a listener to Laravel's logging system. To monitor exceptions, simply use the `Log` facade:

    App::error(function(Exception $exception, $code)
    {
        Log::error($exception);
    });

Your other log messages will also be sent to Rollbar:

    Log::info('Here is some debug information', array('context'));
