# StyleTools\Client

The client class provides methods for generating the javascript required to create a connection between the 📐 Style.Tools browser widget and the CMS. It will add management and stylesheet editing features to the browser widget.

**The client javascript should only be printed for logged in admins.**

The client javascript is tiny and simple. The code is available as Composer package, NPM package and on Github.

### Install via PHP Composer

```bash
composer require styletools/cms-connect-client
```

Package: http://packagist.org/packages/styletools/cms-connect-client

### Install via NPM

```bash
npm install @style.tools/cms-connect --save
```

Package: http://npmjs.com/package/@style.tools/cms-connect

### Install via Github

```bash
git clone https://github.com/style-tools/cms-connect-client
```

Package: https://github.com/style-tools/cms-connect-client

# Methods

## ::load( $ajax_url, $api_params = false, $api_headers = false, $css_sources = false, $js_sources = false )

The load method loads the client javascript and applies configuration to make a (secure) connection with the 📐 Style.Tools browser widget.

### Example from WordPress plugin

The following example shows how a WordPress REST API URL is used as `$ajax_url` and how the client can be configured to add custom HTTP POST variables or HTTP request headers to AJAX requests, for example to authenticate requests.

The client javascript should printed in the `<head>` section of the HTML document.

```php
// 📐 Style.Tools client script
$client_script = StyleTools\Client::load(
    rest_url('styletools/v' . STYLETOOLS_VERSION.'/api/'), // alternative: admin_url('admin-ajax.php')
    false,
    array(
        'X-WP-Nonce' => \wp_create_nonce("wp_rest")
    )
);

print '<script id="style-tools-client">'.$client_script.'</script>';
```

The client javascript includes a function to start the 📐 Style.Tools browser widget:

```javascript
load_styletools(); // start 📐 Style.Tools
```

If you want to be able to instantly start the 📐 Style.Tools browser widget via a link on an other page (e.g. from within an admin panel) you can use the following example code directly after the client javascript which starts the widget using the hash `#style.tools` and then instantly removes the hash via `history.replaceState`.

```javascript
if (document.location.hash==\'#style.tools\'){
  load_styletools();
  try {history.replaceState(null, null, \' \');} catch (e) {}
}
```