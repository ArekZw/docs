# StyleTools\Critical

The critical class provides methods for managing and conditional inlining of Critical CSS.

# Methods

## ::load($condition_groups = false))

The `load` method loads the conditional Critical CSS configuration from the database using key `critical`, processes the conditions and returns the matching Critical CSS in an array containing the concatenated CSS text and an array with individual CSS text + originating condition config.

The optional `$condition_groups` argument enables to load Critical CSS for one or more groups.

### Example

The following example shows how Critical CSS is conditionally loaded from the configuration stored in the database which is then printed in the `<head>` section of the HTML document.

To enable 📐 Style.Tools to automatically recognize the Critical CSS, add `id="critical.css"` to the `<style>` element.

```php
// load critical css
$critical = StyleTools\Critical::load();

// print inline Critical CSS
print '<style id="critical.css">'.$critical.'</style>';
```

## ::get_conditions($name = '')

The `get_conditions` method loads the Critical CSS condition configuration from the database.

An optional `$name` argument enables to load a condition by `name` reference.

## ::loaded_config()

The `loaded_config` method returns the loaded configuration that was used to generate Critical CSS using the `load` method. This method returns an array with the merged critical CSS text as first value and a second value containing an array with individual Critical CSS files and JSON config.

### Data-format returned by method

```javascript
[
	'merged-critical-css-text',
	[
		[
			'critical-css-file-text',
			{ /* JSON config */ }
		]
	]
]
```

### Example

To enable easy editing of the Critical CSS in the 📐 Style.Tools browser widget, you could print the individual Critical CSS `<style>` elements.

```php
// print individual Critical <style> elements for easy editing in 📐 Style.Tools
if (user_is_admin()) {

	// retrieve loaded critical CSS config
	$loaded_critical = Critical::loaded_config();

	if ($loaded_critical && isset($loaded_critical[1])) {
	    foreach ($loaded_critical[1] as $data) {

	    	// $data[0] = critical CSS text
	    	// $data[1] = applicable config JSON

	    	// HTML attributes:
	    	// data-src enables revert from source ability
	    	// data-critical marks the applied critical CSS config by name-reference

	        print '<style data-src="'.str_replace('\'', '&apos;', htmlentities(implode(',', $data[1]['src']), ENT_NOQUOTES, 'utf-8')).'" data-critical="'.str_replace('\'', '&apos;', htmlentities($data[1]['name'], ENT_NOQUOTES, 'utf-8')).'">'.$data[0].'</style>';
	    }
	}
}
```

## ::add_config($config)

The `add_config` method enables to manually add (conditional) Critical CSS config from PHP without depending on the central database configuration, or to extend the database configuration with additional configuration within the CMS.

### Example: add critical config

```php
// load:string
StyleTools\Critical::add_config(array(
    'name' => 'Test',
    'src' => array('critical.css'),
    'weight' => 5,
    'group' => 'x',
)));
```

### Example: add conditional config

```php
// load:string
StyleTools\Critical::add_config(array(
    'weight' => 10,
    'name' => 'Test 2',
    'src' => array('critical-by-condition.css'),
    'conditions' => array(
      array(
        'type' => 'url',
        'pattern' => '/.*/',
        'regex' => true
     )
    )
)));
```

## ::save($config)

The `save` method enables to write Critical CSS condition configuration to the database. The method automatically verifies the configuration using a JSON schema and throws an exception when the JSON is invalid.

### Example: save critical config

```php
$critical_conditions = array(
	array( // first conditional config
		'name' => 'Test',
		'src' => array('critical.css'),
		'weight' => 5,
		'group' => 'x',
		'conditions' => array(
			array(
				'type' => 'url',
				'pattern' => '/.*/',
				'regex' => true
			)
		)
	),
	array( // second conditional config
		'name' => 'Test 2',
		'src' => array('critical-2.css'),
		'weight' => 10,
		'group' => 'y'
	)
);

try {
    // save
    $json = Critical::save($critical_conditions);
} catch (\Exception $err) {
    $error = $err->getMessage();
}
```