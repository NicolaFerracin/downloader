# Downloader

Badly named [npm package](https://www.npmjs.com/package/multiple-urls-images-downloader).

Given a list of URLs, this module will collect all the images on each URL and store them in separate PDF files.

Single steps are:

- asynchronously get HTML content for each URL
- extract image URLs using the given locator function (using [`after-load`](https://www.npmjs.com/package/after-load))
- asynchronously collect all images and merge them to a PDF

## Use case

I wanted to collect images of houses from several real estate websites, as inspiration.

## Libraries used

- [`axios`](https://www.npmjs.com/package/axios): HTTP requests
- [`after-load`](https://www.npmjs.com/package/after-load): full html loading
- [`pdfmake`](https://www.npmjs.com/package/pdfmake): PDF creation

## Install

```
npm -S i multiple-urls-images-downloader
```

## How to use

**NOTE**: You always need to provide the [Roboto fonts](https://fonts.google.com/specimen/Roboto) for the PDF generation (required by [`pdfmake`](https://www.npmjs.com/package/pdfmake)). You can also provide additional custom fonts if you prefer.

```js
const muid = require('multiple-urls-images-downloader');

const config = {
  // Mandatory list of URLs to inspect
  urls: ['url1', 'url2'],

  // Destination dir where to store the PDF files
  // Defaults to './documents'
  dir: './my_dir',

  // Defaults to the url without "/" or ":" or "."
  getTitle: url => url,

  // List of fonts
  fonts: {
    // Mandatory
    Roboto: {
      normal: './fonts/Roboto-Regular.ttf',
      bold: './fonts/Roboto-Medium.ttf',
      italics: './fonts/Roboto-Italic.ttf',
      bolditalics: './fonts/Roboto-MediumItalic.ttf',
    },
    // Optional
    customFont: {
      normal: 'path_to_font.tff',
      bold: 'path_to_font.tff',
      italics: 'path_to_font.tff',
      bolditalics: 'path_to_font.tff',
    },
  },

  // Mandatory
  // Locator function. muid will pass the html string and the $ cheerio object
  // ($ is provided by after-load)
  getImagesHref: (html, $) => {
    const images = [];
    $('img[src^="img/photos"]').each(function() {
      images.push($(this).attr('src'));
    });
    return images;
  },
};

muid(config);
```
