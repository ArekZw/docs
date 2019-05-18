The Critical CSS generator is part of the 📐 Style.Tools browser widget (see [introduction](../README.md)).

There are two different Critical CSS generators:

1. A simple generator inspired by [this snippet](https://gist.github.com/PaulKinlan/6284142).
2. A generator based on [PostCSS](https://github.com/postcss/postcss) CSS parser.

# Generators

## Simple generator

The simple Critical CSS generator is based on a browser-trick that was originally published by Google engineer Paul Kinlan. The snippet uses a Chrome innovation called `getMatchedCSSRules` that was deprecated and has been removed since Chrome 63.

https://gist.github.com/PaulKinlan/6284142

The simple Critical CSS generator uses an enhanced version of the snippet and supports modern browsers via a [polyfill](https://github.com/ovaldi/getMatchedCSSRules). An added feature is the ability to set the viewport to support responsive designs.

### Quality

The quality of the simple critical CSS generator is often not that well but it can provide a good start base for designers that want to manually write pixel perfect Critical CSS.

## Advanced generator

The advanced Critical CSS generator is based on PostCSS parser. The generator can provide a much better result (more accurate and better compression) than the leading tools [Penthouse.js](https://github.com/pocketjoso/penthouse/) and [Critical](https://github.com/addyosmani/critical). Some of the unique features of the generator are the support for multiple viewports (responsive designs) and async script-injected stylesheets. 

The advanced generator can produce pixel-perfect responsive Critical CSS with a single click when configured correctly. The generator can save optimization specialists a lot of time.

# Comparison with other generators

The advanced Critical CSS generator goes a few steps further than Penthouse.js. It uses PostCSS instead of [csstree](https://github.com/csstree/csstree) for improved CSS parsing and it is enhanced in many ways to provide a better quality (more accurate) result, for example by enabling to calculate the Critical CSS based on multiple viewports.

What makes the generator truly different is that it outputs pure and unfiltered Critical CSS and does not apply any compression or optimization techniques. The output of the generator is thereby reliable. No CSS code goes missing without the control of the designer.

Professional CSS code optimization solutions such as [clean-css](https://github.com/jakubpawlowicz/clean-css) are able to provide perfect quality code optimization and compression. The final result is thereby better (better compression and more accurate) than from tools such as Penthouse.js and Critical.

# About Critical CSS

For best website performance, Google PageSpeed Insights [recommends](https://developers.google.com/speed/docs/insights/PrioritizeVisibleContent) inlining the critical (above-the-fold) CSS of a page directly into the HTML. This eliminates additional roundtrips and allows the browser to paint the above-fold experience to a user's screen sooner. This is especially of great impact on mobile devices.

Creating critical CSS has remained difficult. A professional tool named [Penthouse.js](https://github.com/pocketjoso/penthouse) has been the best solution. The tool [Critical](https://github.com/addyosmani/critical) made by a Google engineer uses Penthouse.js under the hood.

The following Github page created by a Google engineer shows a detailed overview of available solutions:

https://github.com/addyosmani/critical-path-css-tools
