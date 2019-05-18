# Modules

The Async Loader can be configured using JSON that is standardized via [JSON schemas](https://github.com/style-tools/async/tree/master/json-schemas).

The Async CSS loader is easy to use and (when using the `API` module) provides onload events. The `$async` load method returns a `then` method that resolves like a Promise when all stylesheets are rendered.

```javascript
$async(
   [/*stylesheets*/],     // string, object or an array of strings or objects
   {/*options*/},         // object
   [/*capture*/],         // string, object or an array of strings or objects 
   {/*capture options*/}  // object
);

// alternative load method for chaining when using the API module
$async.load(/*...*/).load(/*...*/).then(/*...*/)
```

# Examples

### Load 1 stylesheet

```javascript
// simple async loading
$async('sheet.css').then(function() { /* onload */ });
```

### Timed and dependency based loading

The following complex example displays the use of dependency configuration, download and render timing, URL re-basing (compression), `localStorage` cache with `Cache-API` fallback for big stylesheets, HTTP `HEAD` based background cache update, CORS proxy for caching of external stylesheets, XHR configuration and the ability to define a DOM insert target (after or before).

```javascript
// dependencies, timing and global options
$async(
   [  // load 3 stylesheets
      'sheet.css', 
      {
         href:'other-sheet.css',
         dependencies: ['sheet.css'], // wait for sheet.css via dependencies and insert after DOM element
         load_timing: {
            type: 'inview',
            selector: '#footer', // download stylesheet when footer becomes visible within 250 pixels
            offset: -250
         }
      }, 
      {
         href:'mobile-sheet.css',
         target: {
            after: 'meta[charset]'
         },
         load_timing: {
            type: 'media', // download stylesheet based on a media query (also works with viewport changes)
            media: 'screen and (max-width: 600px)'
         }
      }
   ],
   {  // global options applied to all stylesheets
      base: '/long/path/to/css/', // base directory for relative sheet URLs
      cache: {
         type: "localStorage",
         max_size: 10000, // cache only <10kb
         fallback: 'cache-api', // fallback to Cache-API for bigger sheets
         update: {
            head: true, // use HTTP HEAD request to check for 304 - Not Modified
            interval: 86400 // update once per day
         },
         source: ['cssText','xhr','cors'], // default
         cors: {
            proxy: 'https://cors-anywhere.herokuapp.com/', // more proxies on https://gist.github.com/jimmywarting/ac1be6ea0297c16c477e17f8fbe51347
         },
         xhr: {
            headers: {
               "x-special-header": "secret-key" // request header to include in XHR requests
            }
         }
      },
      attributes: { 
         "data-app-sheet": "1" // HTML attribute to add to stylesheet element
      },
      render_timing: 'requestAnimationFrame' // render via requestAnimationFrame
   } 
).then(function() { /* ready */ });
```

### Script-injected stylesheet capture

Many websites contain javascript widgets such as social media or customer review widgets that could severely impact performance scores. One of the culprits is that those widgets inject scripts and stylesheets.

The Async Loader provides a solution to capture such injected scripts and stylesheets and to perform actions such as removal, URL rewriting, HTML attribute modification and timing (e.g. delay or optimize the download).

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
         match: "/domain\\.com/[a-z]{2}/.*/external-sheet.css$/",
         regex: true,
         action: {
            type: "rewrite",
            search: "/^http(s)?:\\/\\/www\\.domain\\.com",
            regex: true,
            replace: "https://cdn.domain.com",
            timing: "requestIdleCallback"
         }
      },
      {
         match: "customer-review.js",
         action: {
            timing: {
               type: 'inview',
               selector: '#customer-review', // load script when customer review widget enters viewport
               offset: -100
            }
         }
      }
   ],
   {
      insert: true // use DOM insert method rewriting
   }
);
```



// chainable
$async
   .on('load',function(sheet, sheetEl){
      //  sheet.css or other-sheet.css loaded
   }) 
   .on('sheet-ref',function() { }) // sheet with ref-name loaded
   .on('sheet.css', function() {}); // sheet with href loaded
   .load({
      href: 'sheet.css', 
      ref: 'sheet-ref'
   })
   .then(function() { }) // sheet.css loaded
   .load('other-sheet.css');
