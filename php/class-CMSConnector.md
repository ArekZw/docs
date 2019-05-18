# StyleTools\CMSConnector

This is the core class.

# Methods

## ::init()

The init method initiates the PHP library and verifies the configuration.

```php
StyleTools\CMSConnector::init(/* CMS config */, /* methods */, /* config store */);
```

### Example

```php
StyleTools\CMSConnector::init(
  array(
      'cms' => array(
          'name' => 'cms-name',
          'title' => 'CMS Name',
          'icon' => 'path/to/logo.svg',

          // links to documentation, also available individually (e.g. critical css, async css help links)
          'help' => array(
              'default' => 'https://docs.style.tools/cms-plugins'
          ),

          // boilerplates tailored to your CMS
          'default_json' => array(
              'critical' => array(),
              'async_css' => array(),
              'async_js' => array()
          )
      ),
      // valid CSS save paths
      'css_root_paths' => array(
          array(
              'path' => $_SERVER['DOCUMENT_ROOT'], // anywhere in document root
              'host' => $_SERVER['HTTP_HOST'] // only for domain
          ),
          array(
              'path' => $_SERVER['DOCUMENT_ROOT'], // anywhere in document root
              'host' => 'cdn.domain.com' // enable editing of stylesheets on CDN
          )
      ),

      'css_path' => 'path/to/public/css/', // css root path (storage directory)
      'critical_css_path' => 'path/to/public/css/critical/', // critical css root path (storage directory)
      'css_url' => 'https://domain.com/css/',
      'cache_path' => 'path/to/styletools/cache/'
  ),
  array(
      /* authentication method */
      'auth' => function () {
        global $CMS;
          return $CMS->is_logged_in(); // is user allowed to edit stylesheets?
      }
  ),
  "StyleTools\ConfigStore_YOUR_CMS" // store config in MySQL or your optimized CMS config store? When omitted, a PHP Opcache based JSON file store is used
);
```