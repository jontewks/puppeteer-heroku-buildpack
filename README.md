# puppeteer-heroku-buildpack

Installs dependencies needed in order to run puppeteer on heroku. Be sure to include `{ args:
['--no-sandbox'] }` in your call to `puppeteer.launch`

### Additional language support
If you need support for Japanese, Chinese, or Korean fonts, a fork of this buildpack has been made to include those as well: https://github.com/CoffeeAndCode/puppeteer-heroku-buildpack
