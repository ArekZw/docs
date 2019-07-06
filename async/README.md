# Introduction

[@style.tools/async](https://npmjs.com/package/@style.tools/async) is lightweight asynchronous CSS and Javascript loader.

```javascript
$async(
   [/*stylesheets*/],	// string, object or an array of strings or objects
   {/*options*/},			// object
   [/*capture*/],			// string, object or an array of strings or objects 
   {/*capture options*/}		// object

   /* 5 to 8th = javascript loader, same config as CSS loader */
   [],{},[],{}
).then(function() { /* ready */ });	

$async.js([],{},[],{}) // direct access to javascript loader
```

### Features

1. Dependency, responsive and timed download and/or render/exec.
2. [in-view](https://github.com/camwiegert/in-view) (element in view) based loading.
3. `requestAnimationFrame` and `requestIdleCallback` (smooth rendering/exec based on CPU)
4. Chainable and events.
5. `localStorage` or [Cache API](https://developer.mozilla.org/en-US/docs/Web/API/Cache) based loading (much faster, see [css-art.com](https://css-art.com))
6. Async injected stylesheet and script capture via `MutationObserver` or DOM insert method rewriting.
7. Fallback via `try {} catch` and `<noscript rel="css">`.
8. Google Closure Compiler with Advanced Optimizations based script compression (IIFE).
9. HTML data-attribute `data-c` based JSON config to enable Content-Security-Policy (CSP) with page-based config.
10. [Performance API](https://developer.mozilla.org/en-US/docs/Web/API/Performance) timings for debugging and optimization.

### Install via npm

```bash
npm install @style.tools/async --save
```

### Install via PHP Composer

```bash
composer require styletools/async
```

The script is optimized to achieve the minimum size possible for above the fold optimization.

```text
async-core.js Size: 2.04 kb (2089 bytes) Gzip: 0.99 kb (1012 bytes).
event-emitter.js Size: 0.47 kb (482 bytes) Gzip: 0.24 kb (245 bytes).
debug.js Size: 0.13 kb (130 bytes) Gzip: 0.13 kb (135 bytes).
css-loader.js Size: 1.07 kb (1094 bytes) Gzip: 0.63 kb (646 bytes).
js-loader.js Size: 1.83 kb (1876 bytes) Gzip: 1.01 kb (1032 bytes).
inline-js.js Size: 0.38 kb (390 bytes) Gzip: 0.27 kb (278 bytes).
rebase.js Size: 0.12 kb (124 bytes) Gzip: 0.12 kb (124 bytes).
regex.js Size: 0.14 kb (142 bytes) Gzip: 0.14 kb (144 bytes).
vendor.js Size: 0.18 kb (187 bytes) Gzip: 0.17 kb (169 bytes).
api.js Size: 0.25 kb (256 bytes) Gzip: 0.18 kb (187 bytes).
dependency.js Size: 0.71 kb (727 bytes) Gzip: 0.41 kb (418 bytes).
timing.js Size: 0.71 kb (726 bytes) Gzip: 0.41 kb (415 bytes).
inview.js Size: 0.92 kb (944 bytes) Gzip: 0.55 kb (566 bytes).
responsive.js Size: 0.26 kb (267 bytes) Gzip: 0.20 kb (201 bytes).
cache.js Size: 1.35 kb (1381 bytes) Gzip: 0.76 kb (776 bytes).
cache-css.js Size: 0.32 kb (325 bytes) Gzip: 0.24 kb (242 bytes).
cache-js.js Size: 0.05 kb (55 bytes) Gzip: 0.07 kb (71 bytes).
localstorage.js Size: 0.43 kb (438 bytes) Gzip: 0.28 kb (284 bytes).
cache-api.js Size: 0.62 kb (632 bytes) Gzip: 0.34 kb (353 bytes).
xhr.js Size: 0.83 kb (849 bytes) Gzip: 0.49 kb (503 bytes).
cache-update.js Size: 0.15 kb (152 bytes) Gzip: 0.13 kb (134 bytes).
capture.js Size: 1.23 kb (1259 bytes) Gzip: 0.71 kb (723 bytes).
capture-observer.js Size: 0.26 kb (263 bytes) Gzip: 0.20 kb (208 bytes).
capture-insert.js Size: 0.34 kb (348 bytes) Gzip: 0.22 kb (224 bytes).
capture-css.js Size: 0.14 kb (141 bytes) Gzip: 0.13 kb (131 bytes).
capture-js.js Size: 0.07 kb (69 bytes) Gzip: 0.08 kb (87 bytes).
attr-config.js Size: 0.29 kb (293 bytes) Gzip: 0.22 kb (229 bytes).
```

---

### JSON config

The configuration of `window.$async` is based on JSON that can be verified using [JSON schemas](https://github.com/style-tools/async/tree/master/json-schemas). Visit [https://json-schema.org](https://json-schema.org) for info about JSON schemas.

A JSON compression innovation enables to compress the JSON using numeric indexes to reduce the size to a minimum. Using a global rebasing configuration, the string-size for individual stylesheet or script URLs can be compressed as well.

HTML data-attribute `data-c` based configuration makes it possible to securely use dynamic page-based configuration with a strict Content-Security-Policy (CSP).

#### Example usage

Non-compressed.

```html
<!-- pre-generated IIFE -->
<script src="async-iife.js"></script>
<script>
	// default (first 4 for css-loader/capture)
	$async(["css/style.css", {"href": "css/extra.css", "load_timing": "domReady"}]);

	// js-loader only (first 4 for js-loader/capture)
	$async(["js/script.css", {"src": "js/extra.jss", "load_timing": "requestIdleCallback"}]);

	// when using css-loader + js-loader, $async accept 8 parameters (last 4 for js-loader/capture)
	$async(0,0,0,0,["js/script.css", {"src": "js/extra.js", "load_timing": "requestIdleCallback"}]);

	// when using css-loader + js-loader, $async.js provides direct access to js-loader/capture
	$async.js(["js/script.js", {"src": "js/extra.js", "load_timing": "requestIdleCallback"}]);

	// direct access to capture config
	$async.capture(
		[/*capture*/],			// string, object or an array of strings or objects 
   		{/*capture options*/}
   	);
	$async.js.capture([],{});
</script>
```

Compressed + using `data-c` attribute config. The attribute accepts a JSON array with 8 values. The first 4 for the css-loader/capture, the last 4 for the js-loader/capture.

```html
<script async src="async-iife.js" data-c='[["css/style.css",{"4":"css/extra.css","21":27}],0,0,0,["js/script.css", {"src": "js/extra.jss", "load_timing": "requestIdleCallback"}]]'></script>
```