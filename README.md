# puppeteer-heroku-buildpack

Installs dependencies needed in order to run puppeteer on heroku. Be sure to include `{ args: ['--no-sandbox'] }` in your call to `puppeteer.launch`.

Puppeteer defaults to `headless: true` in `puppeteer.launch` and this shouldn't be changed. Heroku doesn't have a GUI to show you chrome when running `headless: false` and Heroku will throw an error.

If you want to use puppeteer with firefox instead of chrome, use this buildpack instead: https://github.com/jontewks/heroku-buildpack-puppeteer-firefox

## Usage

To use the latest stable version run:

```sh-session
$ heroku buildpacks:add jontewks/puppeteer
```

The tags in this buildpack are now tied to Heroku stacks that they install dependencies for. The latest at the moment is Heroku-22. This buildpack's tag 22.0.0 is just the dependencies for Heroku-22. This will allow the smallest slug sizes and quickest build times for each stack when we aren't installing any unnecessary dependencies.

To use a specific version (Heroku-22 stack version in this case):

```sh-session
$ heroku buildpacks:add https://github.com/jontewks/puppeteer-heroku-buildpack#22.0.0
```

### Additional language support

If you need support for Japanese, Chinese, or Korean fonts, a fork of this buildpack has been made to include those as well: https://github.com/CoffeeAndCode/puppeteer-heroku-buildpack

## Issues

#### Caching

A common issue that people run into often is a cache issue with heroku. Often when you start seeing errors that chrome won't start and some libraries are missing, you can resolve it by clearing your heroku cache. Instructions for that can be found here: https://help.heroku.com/18PI5RSY/how-do-i-clear-the-build-cache

#### Slug Size

A common issue is running into slug size maximums when using this buildpack. Unfortunately the chromium bundled with Puppeteer requires some packages to be installed on Heroku that are quite large. The list of common missing requirements on Debian for Puppeteer can be found here: https://pptr.dev/troubleshooting, and the list of available packages on each Heroku stack by default can be found here: https://devcenter.heroku.com/articles/stack-packages#installed-ubuntu-packages. This buildpack only installs things in the first list that aren't found in the second list and their sizes are as follows:
| Package | Size (mb) |
| ------- | ---- |
| `fonts-liberation` | 2.1 |
| `libappindicator3-1` | 55.2 |
| `libasound2` | 2.4 |
| `libatk-bridge2.0-0` | 3.9 |
| `libatk1.0-0` | 0.2 |
| `libgbm1` | 0.4 |
| `libgtk-3-0` | 54.8 |
| `libnspr4` | 0.3 |
| `libnss3` | 4.2 |
| `libx11-xcb1` | 0.1 |
| `libxcomposite1` | 0.03 |
| `libxcursor1` | 0.1 |
| `libxdamage1` | 0.03 |
| `libxfixes3` | 0.05 |
| `libxi6` | 0.1 |
| `libxrandr2` | 0.07 |
| `libxss1` | 0.03 |
| `libxtst6` | 0.05 |
| `xdg-utils` | 344 😱 |

Some of these are quite large but unfortunately there is nothing we can do about it at the moment if we are following Puppeteer's list of requirements exactly.

I did try to deploy a very minimal Puppeteer app on Heroku without installing `xdg-utils` since that was the largest library by far, and to my surprise the app still worked. I'm assuming there will be some functionality within Puppeteer that won't work due to the missing library, but perhaps if you aren't doing anything that requires that library, you can get away with this other release that doesn't include that library to dramatically reduce your slug size. It's an experiment though so don't use this for very critical things as I have no idea when it might stop working for you.

https://github.com/jontewks/puppeteer-heroku-buildpack/releases/tag/22.0.0-no-xdg-utils

If you are still running into any issues with this buildpack after doing the above, please open an issue on this repo and/or submit a PR that resolves it. Different versions of chrome have different dependencies and so some issues can creep in without me knowing. Thanks!
