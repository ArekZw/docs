# Installation

[@style.tools/async](https://npmjs.com/package/@style.tools/async) is available on NPM and as a PHP Composer package.

### Install via NPM

```bash
npm install @style.tools/async --save
```

Package: https://npmjs.com/package/@style.tools/async

### Install via PHP Composer

```bash
composer require styletools/async
```

Package: https://packagist.org/packages/styletools/async

# Setup

`$async` has been designed for `above-the-fold` optimization. The script is compiled into individual modules via [Google Closure Compiler](https://developers.google.com/closure/compiler/) that does not just compress javascript but also [optimizes it for speed](https://developers.google.com/closure/compiler/). The individual modules can be concatenated without the use of a module loader.

## IIFE Generator

An IIFE is a pre-concatenated selection of `$async` modules that is additionally optimized using Google Closure Compiler with mode Advanced Optimizations. IIFE or [Immediately-invoked Function Expressions](https://medium.com/@vvkchandra/essential-javascript-mastering-immediately-invoked-function-expressions-67791338ddc6) is a coding pattern for loading a script. An IIFE can be used in the browser safely.

For more documentation, see [IIFE Generator](./iife-generator.md)

## Manual concatenation

The Google Closure Compiler module architecture enables to concatenated a selection of modules without the use of a module loader. You can simply wrap the module source text in a IIFE and include it safely in the HTML document.

### Example concatenation

```php
<?php

// include a selection of $async modules inline
echo "<script>(function(){"; // wrap in IIFE
readfile('vendor/styletools/async/dist/async-core.js');
readfile('vendor/styletools/async/dist/css-loader.js'); 
readfile('vendor/styletools/async/dist/cache.js'); // cache module
readfile('vendor/styletools/async/dist/localstorage.js'); // localStorage module
echo "})();</script>";
?>
```

### Example using [📐 Style.Tools PHP library](https://github.com/style-tools/php-cms-connector/)

```php
<?php

// CSS Loader config 
$async_config = array(
	'load' => array( 'sheet.css' ),
	'load_options' => array( 'load_timing' => 'requestAnimationFrame' )
);

// discover modules via config
$modules = StyleTools\AsyncCSS::discover_modules($async_config);

// manually add some modules
$modules[] = 'api'; // public API and events
$modules[] = 'attr-config'; // data-c HTML attribute config

// generate IIFE
$iife_options = array(
    'cache' => true,
    'compress' => true,
    'mode' => 'unary'
);
$module_iife = StyleTools\IIFE::generate($modules, $iife_options);

// compress JSON config
$compressed_config = StyleTools\AsyncCSS::compress($async_css_config, null, false, true);

// inline $async script
echo '<script data-c=\''.(($compressed_config) ? $compressed_config : '').'\'>'.$module_iife.'</script>';
?>
```