# Conditional loading

Conditional loading enables to define conditions for which to inline critical CSS or to load CSS or javascript asynchronously with any given configuration (e.g. load timing). Conditions can range from a page URL to browser environment information such as the viewport size or the mobile device type of the user. 

The PHP library provides three condition types:

1. URL matching
2. Request header matching
3. PHP method matching

Using these conditions, any type of matching is possible. PHP logic enables to do anything on the server side while the easy access to request headers makes it possible to efficiently match based on the browser environment, for example to target or exclude specific browsers or mobile devices.

Conditions can be negative or positive. They can exclude or include browsers or pages for a given configuration.

## Condition parameters

### `weight` parameter

The `weight` parameter determines the loading order (a higher weight is loaded first).

### `group` parameter

The `group` parameter determines the merging behaviour. The groups defined in the first matching condition will be used to match against any other conditions. When no `group` is provided the group name is set to wild card `*` (match any group).

The `group` parameter has a double purpose. The `group` parameter can define one or more groups as a string or a array of strings or define matching rules for applying a configuration on the basis of groups from other conditions, for example to append a condition to any other group or based on a match pattern (string or regular expression).

The special string `*` is a wildcard to match any group. It would result in the match object `{match: "/.*/", regex: true}`.

#### Examples

```json
{
  "group": "group-name"
}
```

The following configuration will define both a group and would append itself when a higher weight condition with `group-name` is loaded.

```json
{
  "group": ["group-x", {"match":"group-name"}]
}
```

The first matching group will determine what other conditions are loaded. If a condition does not have a matching group, it will be ignored.

### `conditions` parameter

The `conditions` parameter defines the condition configuration.

#### Example

The following conditions match on two URLs.

```json
{
  "conditions": [
      {
        "type": "url",
        "pattern": "path/file"
      },
      {
        "type": "url",
        "pattern": "path/other-file"
      }
  ]
}
```

### Match any vs Match all

The condition JSON configuration array provides a `match any` or `match all` functionality on the basis of array depth level. The first array depth level provides a `match any` functionality while a second array depth level provides `match all` functionality. If you want to match multiple conditions as a group then you should configure the rules with double array brackets: `[[ ... ]]`.

The following example matches only for URLs containing `path/file` and browsers with a `User-Agent` that contains `Chrome`.

```json
{
  "conditions": [
    [
      {
        "type": "url",
        "pattern": "path/file"
      },
      {
        "type": "header",
        "name": "user-agent",
        "pattern": "Chrome"
      }
    ]
  ]
}
```

### URL matching

URLs can be matched using a simple string or using a regular expression.

```json
{
  "type": "url",
  "pattern": "/path([0-9]+)/i",
  "regex": true
}
```

### Request header matching

Request headers can be matched using a simple string, using a regular expression or using a comparison operator (e.g. `<>=`).

```json
{
  "type": "header",
  "name": "user-agent",
  "pattern": "/chrome([0-9]+)/i",
  "regex": true
}
```

Match using a operator comparison.

```json
{
  "type": "header",
  "name": "content-length",
  "pattern": {
    "operator": "<",
    "value": 2048
  }
}
```

### PHP method matching

Conditions can be based on PHP methods. It is possible to define both method arguments and result evaluation. By default, a result of `true` is expected. It is also possible to evaluate the result based on a array of values.

For simple usage by average users, you likely need just the `method` and optionally the `arguments` parameters.

#### Argument based evaluation with boolean result.

```json
{
  "type": "method",
  "method": "is_product_page",
  "arguments": [ 1,5,19 ]
}
```

#### Result evaluation. 

```json
{
  "type": "method",
  "method": "get_product_id",
  "result": [ 1,5,19, "string" ]
}
```

_Note: The condition match system is optimized for boolean result based matching. Result evaluation may cause a performance hit with many thousands of conditions._

## Example of PHP method condition

The following example shows how to conditionally load Critical CSS for product pages with a specific ID.

```php
// match product pages
if (!function_exists('st_is_product')) {
  function st_is_product($page_id)
  {
    global $CMS;
    return $CMS->is_product_page($page_id);
  }
}

// load configured critical CSS
$critical = StyleTools\Critical::load();

// inline critical CSS
print '<style id="critical.css">'.$critical[0].'</style>';
```

The following Critical CSS condition would be loaded if the product page matches `page-id`.

```json
{
  "name": "product",
  "weight": 1,
  "conditions": [
    {
      "type": "method",
      "method": "st_is_product",
      "arguments": [
        "page-id"
      ]
    }
  ]
}
```

## Example of browser conditions


### Example: `chrome.css`, `special-page.css` and `default.css`

The following example shows how to conditionally apply an Async CSS loading configuration.

```json
[{
    "name": "match pages",
    "weight": 5,
    "group": "special-pages",
    "load": "special-pages.css",
    "conditions": [
        [{
            "type": "method",
            "method": "is_page",
            "params": [1, 5, 19]
        }]
    ]
}, {
    "name": "match chrome browser",
    "weight": 2,
    "group": "*",
    "load": { "href": "chrome.css" , "render_timing": "requestAnimationFrame" },
    "conditions": [
        [{
            "type": "header",
            "name": "user-agent",
            "pattern": "Chrome"
        }]
    ]
}, {
    "name": "match any other page",
    "weight": 1,
    "load": "default.css"
}]
```