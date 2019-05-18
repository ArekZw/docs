# Simple

The simple Critical CSS generator is based on a browser-trick that was originally published by Google engineer Paul Kinlan. The snippet uses a Chrome innovation called `getMatchedCSSRules` that was deprecated and has been removed since Chrome 63.

https://gist.github.com/PaulKinlan/6284142

The simple Critical CSS generator uses an enhanced version of the snippet and supports modern browsers via a [polyfill](https://github.com/ovaldi/getMatchedCSSRules). An added feature is the ability to set the viewport to support responsive designs.

### Quality

The quality of the simple critical CSS generator is often not that well but it can provide a good start base for designers that want to manually write pixel perfect Critical CSS.