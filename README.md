Heroku buildpack: Coffeescript
==============================

This is a [Heroku buildpack](http://devcenter.heroku.com/articles/buildpacks) for [Coffeescript](http://coffeescript.org/) apps, and is compatible with [Cedar-14 stack](https://blog.heroku.com/archives/2014/11/4/cedar_14_now_generally_available). It uses [NPM](http://npmjs.org/) and [SCons](http://www.scons.org/). It was forked from the [Heroku Node.js buildpack](https://github.com/heroku/heroku-buildpack-nodejs). It is also loosely based on the [original coffeescript buildpack](https://github.com/aergonaut/heroku-buildpack-coffeescript) by [Chris Fung](https://github.com/aergonaut), which did not work out-of-the-box with Cedar-14.

Usage
-----

Example usage:

    $ ls
    Procfile  package.json  src

    $ ls src
    app.coffee

    $ heroku create --stack cedar --buildpack https://github.com/sailrish/heroku-buildpack-coffee.git

    $ git push heroku master
    ...
    Compressing source files... done.
    Building source:

    -----> Fetching custom git buildpack... done
    -----> Coffeescript app detected

    -----> Creating runtime environment

           NPM_CONFIG_LOGLEVEL=error
           NPM_CONFIG_PRODUCTION=true
           NODE_ENV=production
           NODE_MODULES_CACHE=true

    -----> Installing binaries
           engines.node (package.json):  0.10.x
           engines.npm (package.json):   1.3.x

           Resolving node version 0.10.x via semver.io...
           Downloading and installing node 0.10.40...
           Resolving npm version 1.3.x via semver.io...
           Downloading and installing npm 1.3.26 (replacing version 1.4.28)...
           npm WARN package.json github-url-from-git@1.1.1 No repository field.

    -----> Restoring cache
           Skipping cache (new runtime signature)

    -----> Building dependencies
           Pruning any extraneous modules
           Installing node modules (package.json)
    ...
    ...
    -----> Compiling coffeescript source
           Source compiled to target

    -----> Caching build
           Clearing previous node cache
           Saving 1 cacheDirectories (default):
           - node_modules

    -----> Build succeeded!
    ...
    ...
    -----> Discovering process types
           Procfile declares types -> web

    -----> Compressing... done, 24.1MB
    -----> Launching... done, v10
    ...

The buildpack will detect your app as Coffeescript if it detects a file matching `src/*.coffee` in your project root.  It will use NPM to install your dependencies, and vendors a version of the Node.js runtime into your slug.  The `node_modules` directory will be cached between builds to allow for faster NPM install time.

You must include Coffeescript in your `package.json`. The buildpack does not install Coffeescript automatically in order to allow you to specify your own Coffeescript version.

Compiled javascript is written to the `target` directory in the slug. Your `Procfile` should reference these compiled files like so:

    web: node target/app.js

Node.js and npm versions
------------------------

You can specify the versions of Node.js and npm your application requires using `package.json`

    {
      "name": "myapp",
      "version": "0.0.1",
      "engines": {
        "node": "0.10.x",
        "npm": "1.3.x"
      }
    }

To list the available versions of Node.js and npm, see these manifests:

* http://heroku-buildpack-nodejs.s3.amazonaws.com/manifest.nodejs
* http://heroku-buildpack-nodejs.s3.amazonaws.com/manifest.npm

