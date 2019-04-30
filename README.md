# Downloader

Given a list of URLs, this Node.js script will:

- asynchronously get content for each URL
- extract image URLs using the given locator function
- asynchronously collect all images and merge them to a PDF

## Use case

I wanted to collect images of houses from real estate websites, as inspiration.

Can be used to collect any type of content from a given list of URLs.

## Libraries used

- axios: HTTP requests
- after-load: full html loading
- pdfmake: PDF creation

## Install

```
npm -S i nested-content-downloader
```

## How to use

```js
const ncd = require('nested-content-downloader');

const config = {
  // mandatory
  urls: ['url1', 'url2'],

  // destination dir. Defaults to './documents'
  dir: './my_dir',

  // defaults to the url without /:.
  getTitle: url => url,

  // defaults to Roboto fonts. Used for the text content
  fonts: {
    Roboto: {
      normal: 'fonts/Roboto-Regular.ttf',
      bold: 'fonts/Roboto-Medium.ttf',
      italics: 'fonts/Roboto-Italic.ttf',
      bolditalics: 'fonts/Roboto-MediumItalic.ttf'
    }
  },

  /**
   * Mandatory
   * Locator function. ncd will pass the html string and the $ cheerio object
   * ($ is provided by after-load)
   */
  getImagesHref: (html, $) => {
    const images = [];
    $('img[src^="img/photos"]').each(function() {
      images.push($(this).attr('src'));
    });
    return images;
  }
};

ncd(config);
```
