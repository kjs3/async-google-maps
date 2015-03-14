# async-google-maps
Asynchronous loading and creation of google maps

[![Build Status](https://travis-ci.org/zpratt/async-google-maps.svg)](https://travis-ci.org/zpratt/async-google-maps)

## Why use this module?

I wrote this module to have the simplest possible public API and to handle the common case of waiting to add things to the map until it is `idle`. It is also targeted at making the creation of multiple map instances easier to manage, since each call to `create` will return a promise. There is [another library](https://github.com/sakren/node-google-maps) you can use if you don't like mine.

Additionally, I have focused on code readability, test quality, and attempted to make this an easy module to TDD with as a dependency. You can easily stub the API, instead of having to verify properties of the module itself. TDD is a passion of mine.

### Not fully baked yet

I have yet to include the options for the business version, but I expect to include that in the next release.

## Running tests

This module is 100% test-driven. Please feel free to run the tests and critique them.

1. Install io.js (needed for jsdom)
- `npm i`
- `npm test`

## Usage

This is a pure commonjs module with no production dependencies. I recommend using either [webpack](http://webpack.github.io/docs/) or [browserify](https://github.com/substack/node-browserify) to build.

```javascript

var MapLoader = require('async-google-maps');

MapLoader.load({
    key: 'some-api-key',
    version: '3.20'
});

MapLoader.create(
    document.querySelector('.map-container'),
    {
        center: {
            lat: 40.0,
            lng: -90.0
        },
        zoom: 7
    }
);
```

### Promises

This library leverages the ES6 promise implementation, which is currently [available in most modern browsers](http://caniuse.com/#feat=promises). If you wish to use it with browsers that do not support the ES6 promise implementation, then I recommend using [a polyfill](https://github.com/jakearchibald/es6-promise).

### Why?

I have intentionally limited the production dependencies of this module, so you are not forced to use a particular library.

## API

### MapLoader.load

Asynchronously loads the google maps javascript library, given the supplied options. Returns a promise that will be resolved once the google maps loader has finished. Once the promise resolves, it is safe to reference anything under the `google.maps` namespace. This method should only be called once for a given application.

### MapLoader.create

Creates a map instance given the supplied options. The options will be passed into the `google.maps.Map` constructor, therefore, all options from the [google maps api](https://developers.google.com/maps/documentation/javascript/reference#MapOptions) can be used. This function returns a promise which will be resolved once the newly created map instance is in the `idle` state, which is the point at which overlays, markers, and geometries can be added to the map.
