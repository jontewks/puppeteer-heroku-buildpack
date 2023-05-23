# puppeteer-heroku-buildpack

## IMPORTANT

Puppeteer from v19 and later changed how the install of chromium is cached. In order to get it to work correctly with heroku, you need to add the following to your `scripts` object in your `package.json`:

```js
"scripts": {
  ...
  "heroku-postbuild": "mkdir ./.cache && mv /app/.cache/puppeteer ./.cache"
},
```

Without the above, heroku will not include the new cache directory that puppeteer uses, and your app will fail saying it cannot find chromium.

## Info

Installs dependencies needed in order to run puppeteer on heroku. Be sure to include `{ args: ['--no-sandbox'] }` in your call to `puppeteer.launch`.

Puppeteer defaults to `headless: true` in `puppeteer.launch` and this shouldn't be changed. Heroku doesn't have a GUI to show you chrome when running `headless: false` and Heroku will throw an error.

If you want to use puppeteer with firefox instead of chrome, use this buildpack instead: https://github.com/jontewks/heroku-buildpack-puppeteer-firefox

## Usage

To use the latest stable version run:

```sh-session
$ heroku buildpacks:add jontewks/puppeteer
```

You can also just add this repo's github url into your buildpacks through the UI.

### Additional language support

If you need support for Japanese, Chinese, or Korean fonts, a fork of this buildpack has been made to include those as well: https://github.com/gnuletik/puppeteer-heroku-buildpack-fonts

## Issues

#### Caching

A common issue that people run into often is a cache issue with heroku. Often when you start seeing errors that chrome won't start and some libraries are missing, you can resolve it by clearing your heroku cache. Instructions for that can be found here: https://help.heroku.com/18PI5RSY/how-do-i-clear-the-build-cache

If you are still running into any issues with this buildpack after doing the above, please open an issue on this repo and/or submit a PR that resolves it. Different versions of chrome have different dependencies and so some issues can creep in without me knowing. Thanks!
