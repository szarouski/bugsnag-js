Bugsnag Notifier for JavaScript (Beta)
======================================

The Bugsnag Notifier for JavaScript gives you instant notification of errors and
exceptions in your website's JavaScript code.

Bugsnag's JavaScript notifier is incredibly small, and has no external
dependencies (not even jQuery!) so you can safely use it on any website.

This notifier is currently in beta, we're working on improving automatic
error capturing and grouping very soon.

[Bugsnag](https://bugsnag.com) captures errors in real-time from your web,
mobile and desktop applications, helping you to understand and resolve them
as fast as possible. [Create a free account](https://bugsnag.com) to start
capturing errors from your applications.


How to Install
--------------

Include `bugsnag.js` from our CDN in the `<head>` tag of your website, before
any other `<script>` tags.

```html
<script src="//d2wy8f7a9ursnm.cloudfront.net/bugsnag-2.0.1.min.js"
        data-apikey="YOUR-API-KEY-HERE"></script>
```

Make sure to set your Bugsnag API key in the `data-apikey` attribute on the
script tag, or manually set [Bugsnag.apiKey](#apikey).


Sending Caught Exceptions or Custom Errors
------------------------------------------

You can easily tell Bugsnag about caught exceptions by calling
`Bugsnag.notifyException`:

```javascript
try {
  // Some code which might throw an exception
} catch (e) {
  Bugsnag.notifyException(e);
}
```

Since many exceptions in JavaScript are named simply `Error`, we also allow
you to provide a custom error name when calling `notifyException`:

```javascript
try {
  // Some code which might throw an exception
} catch (e) {
  Bugsnag.notifyException(e, "CustomErrorName");
}
```

You can also send custom errors to Bugsnag without any exception,
by calling `Bugsnag.notify`:

```javascript
Bugsnag.notify("ErrorName", "Something bad happened here");
```

Both of these functions can also be passed an optional `metaData` object as
the last parameter, which should take the same format as [metaData](#metadata)
described below.


noConflict Support
------------------

Bugsnag has a `noConflict` function for removing itself from the `window` object
and restoring the previous binding. This is intended for use in environments
where developers can't assume that Bugsnag isn't in use already (such as 3rd
party embedded javascript) and want to control what gets reported to their
Bugsnag account.

The object returned from `noConflict()` is the full Bugsnag object so can be
used in the same way:

```javascript
var myBugsnag = Bugsnag.noConflict();
// window.Bugsnag now is bound to what is was before the bugsnag script was
added to the DOM
myBugsnag.apiKey = "my-special-api-key";
try {
  // highly volatile code
} catch (e) {
  myBugsnag.notifyException(e, "OhNoes");
}
```


Browser Support
---------------

Some browsers, notably IE9 and below, don't support stacktraces on exceptions.
In these situations we'll attempt to construct an approximate stacktrace,
which will unfortunately not contain URL or line number information.


Additional Configuration
------------------------

###apiKey

Set your Bugsnag API key. You can find your API key on your dashboard.

```html
<script src="//d2wy8f7a9ursnm.cloudfront.net/bugsnag-2.0.1.min.js"
        data-apikey="YOUR-API-KEY-HERE"></script>
```

In situations where Bugsnag is not in its own script tag, you can set
this with:

```javascript
Bugsnag.apiKey = "YOUR-API-KEY-HERE";
```

###autoNotify

By default, we will automatically notify Bugsnag of any JavaScript errors that
get sent to `window.onerror`. If you want to stop this from happening, you can
set `autoNotify` to `false`:

```html
<script src="//d2wy8f7a9ursnm.cloudfront.net/bugsnag-2.0.1.min.js"
        data-apikey="YOUR-API-KEY-HERE"
        data-autonotify="false"></script>
```

In situations where Bugsnag is not in its own script tag, you can set
this with:

```javascript
Bugsnag.autoNotify = false;
```

###metaData

Set additional meta-data to send to Bugsnag with every error. You can use this
to add custom tabs of data to each error on your Bugsnag dashboard.

This function should return an object of objects, the outer object should
represent the "tabs" to display on your Bugsnag dashboard, and the inner
objects should be the values to display on each tab, for example:

```javascript
Bugsnag.metaData = {
  user: {
    name: "James",
    email: "james@example.com"
  }
};
```

###releaseStage

If you would like to distinguish between errors that happen in different
stages of the application release process (development, production, etc)
you can set the `releaseStage` that is reported to Bugsnag.

```javascript
Bugsnag.releaseStage = "development";
```

By default this is set to be "production".

###notifyReleaseStages

By default, we will only notify Bugsnag of errors that happen when your
`releaseStage` is set to be "production". If you would like to change which
release stages notify Bugsnag of errors you can set `notifyReleaseStages`:

```javascript
Bugsnag.notifyReleaseStages = ["development", "production"];
```


###beforeNotify

To have more fine grained control over what errors are sent to Bugsnag, you can 
implement a `beforeNotify` function. If you want to halt the notification completely,
return `false` from this function. You can also add metaData by editing the `metaData`
parameter.

```javascript
Bugsnag.beforeNotify = function(error, metaData) {
  // Example: Only notify Bugsnag of errors in `app.js` or `vendor.js` files
  var match = error.file.match(/app\.js|vendor\.js/i);
  return !!(match && match[0].length > 0);
}
```

The error parameter contains name, message, file and lineNumber fields that contain
information about the error that is being notified.

Reporting Bugs or Feature Requests
----------------------------------

Please report any bugs or feature requests on the github issues page for this
project here:

<https://github.com/bugsnag/bugsnag-js/issues>


Contributing
------------

-   [Fork](https://help.github.com/articles/fork-a-repo) the [notifier on github](https://github.com/bugsnag/bugsnag-js)
-   Edit only `src/bugsnag.js`. The files in `dist` and `docs` are autogenerated.
-   Make sure your changes support older browsers, avoid any [unsupported methods](http://kangax.github.com/es5-compat-table/#showold)
-   Make sure all tests pass by building (`npm install`, `grunt`), and tests are passing (`grunt test`)
-   Commit only changes you make in `test` and `src`, please don't commit `dist` or `doc`
-   Commit and push until you are happy with your contribution
-   [Make a pull request](https://help.github.com/articles/using-pull-requests)
-   Thanks!


License
-------

The Bugsnag JavaScript notifier is free software released under the MIT License.
See [LICENSE.txt](https://github.com/bugsnag/bugsnag-js/blob/master/LICENSE.txt) for details.
