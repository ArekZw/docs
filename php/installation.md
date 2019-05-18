# Installation

## Install via PHP Composer

```bash
composer require styletools/cms-connector
```

Package: https://packagist.org/packages/styletools/cms-connector

## Auto-load via composer

```php
// composer autoloader
require_once 'vendor/autoload.php';

// access via namespace StyleTools\...

// example: generate an GCC compressed IIFE
$iife = StyleTools\AsyncCSS::iife($modules, $iife_options);
```