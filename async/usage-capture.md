# Script-injected stylesheet capture

The `capture` solution enables to modify stylesheets that are inserted via social media widgets, customer review widgets or other origins that are out of the direct control of the optimization specialist. 

For example, a customer review widget may inject a heavy stylesheet that hurts the performance scores. 

Using the capture module it is possible to:

1. Remove the stylesheet.
2. Rewrite (proxy) the stylesheet (for example to optimize the caching headers).
3. Rewrite HTML attributes (e.g. add or remove `data-*` or `rel="noopener"` attributes).
4. Load async including options such as rebasing and timed loading and timed render.

The capture module provides the following public method:

```javascript
$async.capture([/*capture list*/],{/*options*/});
```

You can also define the capture configuration as 3th and 4th argument via `$async`.

```javascript
$async(/**/,/**/,[/*capture list*/],{/*options*/});
```

# Example

The following example shows how to delete, modify and optimize the loading of several async script-injected stylesheets and scripts.

```javascript
// capture and remove async script-injected sheet
$async.capture(
   [
      {
         match: "bloated-sheet.css",
         action: {
            "type": "remove"
         }
      },
      {
         match: "bloated-script.js",
         action: {
            "type": "remove"
         }
      },
      {
         match: "external-widget.com",
         action: {
            type: "rewrite",
            search: '/^.*cdn\.external-widget\.com/(.*).css$',
            regex: true,
            replace: "/local-nginx-proxy/$1.css", // use simple Nginx @upstream {} 
            async: { "load_timing": "requestIdleCallback" }
         }
      },
      {
         match: "customer-review.js",
         action: {
            async: { 
               "load_timing": {
                  type: 'inview',
                  selector: '#customer-review', // load script when customer review widget enters viewport
                  offset: -100
               }
            }
         }
      }
   ],
   {
      insert: true // use DOM insert method rewriting
   }
);
```

# Capture Methods

The capture module provides two child-modules with a different capture technology.

1. [MutationObserver](https://developer.mozilla.org/nl/docs/Web/API/MutationObserver)
2. DOM insert method rewriting

Both technologies perform differently. DOM insert method rewriting enables to prevent stylesheets from being downloaded while the MutationObserver will successfully prevent the loading of a stylesheet or script but may not prevent a download.

It is possible to use both DOM insert method rewriting and DOM observer. 

# Configuration

The configuration of the CSS loader is available in a JSON schema.

https://github.com/style-tools/async/tree/master/json-schemas/capture.json

## Capture Match Rules

The first argument of `$async.capture()` is an array with objects that contain the following parameters.

| Module                         | Description     |
|--------------------------------|-----------------|
| `type`                | Match type: `css`, `js` or `all`. Default: `all` |
| `mode`                | Mode (environment to apply capture): `cms`, `dom` or `all`. Default: `all` |
| `match`                 | A match pattern (string). |
| `regex`                 | Enable regular expression pattern. |
| `match_type`                 | URL or node (`outerHTML`) based matching. Default: URL. |
| `action`                 | Object |
| `action.type`                | Action type: `rewrite` or `remove` |
| `action.search`                 | Search pattern (for rewrite). When omitted the full URL is replaced. |
| `action.regex`                 | Enable regular expression search pattern. |
| `action.replace`                 | Replace string, e.g. `/$1.css` (regex) |
| `action.attributes`                 | Attributes to add to the DOM node (e.g. `{ "data-attr": "custom" }`) |
| `action.async`                 | Load the captured sheet asynchronously. `true` or a config object with `load_timing`, `render_timing`, `cache` etc. (see [Async Loader](./usage.md#load-configuration) config). **Note:** When using CMS capture, async loaded sheets are added to the `data-c` config which applies the global async load options. |

### Example

```json
{
   "type": "css",
   "match": "external-widget.com",
   "action": {
      "type": "rewrite",
      "search": '/^.*cdn\.external-widget\.com/(.*).css$',
      "regex": true,
      "replace": "/local-nginx-proxy/$1.css",
      "attributes": {
         "data-cdn": "external-widget.com"
      },
      "async": {
         "target": {
            "after": "media[charset]"
         },
         "load_timing": {
            "type": "requestAnimationFrame",
            "frame": 2
         }
      }
   }
}
```

## Capture Options

The options enable to configure the two different capture technologies.

| Module                         | Description     |
|--------------------------------|-----------------|
| `insert`                 | Enable DOM insert method rewrite based capture. `true/false` or an object. |
| `insert.appendChild`                 | Rewrite `appendChild` on `rootNodes`  |
| `insert.insertBefore`                 | Rewrite `insertBefore` on `rootNodes`  |
| `insert.rootNodes`                 | DOM root elements to rewrite methods on. Default: `["Element", "Document"]`  |
| `observer`                 | Enable MutationObserver based capture. |