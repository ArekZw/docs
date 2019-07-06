# FEO widget

A free Frontend Optimization (FEO) widget that works offline and for pages behind a login.

- critical CSS generator
- CSS editor
- CSS beautify
- [clean-css](https://github.com/jakubpawlowicz/clean-css) optimization
- repair broken CSS
- remove unused CSS
- remove duplicate CSS
- extract unused and duplicate CSS (for automated unused CSS removal)
- PostCSS plugins: autoprefixer and more
- CSS lint
- CSS analysis

The widget is a small piece of Javascript code that can be added to the browser bookmarks bar via the URL prefix `javascript:`.

You can copy & paste the following code in the location field of a browser bookmark:

```javascript
javascript:(function(r,a,k,l,f,g,b,m){function n(c,b,d){a.open();b&&(r.onmessage=b);d&&a.addEventListener("securitypolicyviolation",d);a.write(c);a.close()}f="https://style.tools/";g="Style.Tools";var c=a.createElement("script");c.src=f+"x.js";c.onerror=function(){function p(d){if(c=d?d.violatedDirective:0){if("script-src"==c||m)return;m=1;b&&l(b)}if(!q){var h=f+"#"+a.location;a.getElementById("e").innerHTML='<h2 style="color:red;">'+g+(c?' blocked by CSP <font color="blue">'+c+"</font>":" failed to load")+
'.</h2><h3>Redirecting <a href="'+h+'">'+h+"</a>...</h3>";b=k(function(){a.location.href=h},3E3)}}var q;n("<h2>Loading "+g+" via Service Worker...</h2><iframe src="+f+'go height=50></iframe><p id="e"></p>',function(a){q=1;b&&l(b);n("<script>"+a.data+"\x3c/script>")},p);b=k(p,2E3)};a.head.appendChild(c)})(window,document,setTimeout,clearTimeout);
```

Source: https://github.com/style-tools/widget/blob/master/widget.js (849 Bytes)

You can also copy & paste the code in the browser console.

The widget provides localhost and offline support via a Service Worker on domain https://style.tools/. You can use many of the features locally without an internet connection. The cache for preferences is also preserved across domains which makes it efficient when working on multiple websites.

# Creating a bookmark

You can use the bookmark functionality of your browser to create a new bookmark. The short cut in Google Chrome and Firefox is `Ctrl-D`.

![Add a new bookmark](./gitbook/images/add-bookmark.png)

Once the bookmark is added, you can drag it to the bookmarks bar for quick access.

![📐 Style.Tools bookmark](./gitbook/images/browser-bookmark.png)

## Drag & drop to bookmarks bar

📐 Style.Tools provides a drag & drop link via the logo (top left). It will automatically create the correct widget link in the bookmarks.

![Drag & drop bookmark from 📐 Style.Tools](./gitbook/images/drag-bookmark.png)
