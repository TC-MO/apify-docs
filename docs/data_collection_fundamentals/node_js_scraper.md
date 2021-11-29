---
title: Node.js scraper
description: Learn how to use JavaScript and Node.js to create a web scraper, plus take advantage of the cheerio and got-scraping libraries to make your job easier.
menuWeight: 19.6
paths:
    - data-collection-fundamentals/node-js-scraper
---

# [](#collecting-data-with-node) Collecting data with Node.js

Finally we have everything ready to start scraping! Yes, the setup is a bit daunting, but luckily, you only have to do it once. We have our project, we have our `main.js` file, so let's add some code to it.

## [](#downloading-html) Downloading HTML

We will use the `got-scraping` library to download the HTML of the [Alexa Top Sites index](https://www.alexa.com/topsites). Careful, the `import` statement is a little different than in the previous chapter.

```js
// main.js
import { gotScraping } from 'got-scraping';

const response = await gotScraping('https://www.alexa.com/topsites');
const html = response.body;
console.log(html);
```

Now run the script (using `node main.js`). After a brief moment, you should see the page's HTML printed in your terminal.

> `gotScraping` is an `async` function and the `await` keyword is used to pause execution of the script until it returns the `response`. [Learn more](https://javascript.info/async-await) about `async` and `await`.

## [](#parsing-html) Parsing HTML

Having the HTML printed to the terminal is not very helpful. To collect the data, we first have to parse it. Thanks to parsing, we will be able to query the HTML elements similarly to the way we did it in browser in the [Collecting Data with DevTools]({{@link data_collection_fundamentals/using_devtools.md}}) chapters. To parse HTML, we load it into the `cheerio` library.

```js
// main.js
import { gotScraping } from 'got-scraping';
import cheerio from 'cheerio';

const response = await gotScraping('https://www.alexa.com/topsites');
const html = response.body;

const $ = cheerio.load(html);
const titleElement = $('title');
const titleText = titleElement.text();
console.log(titleText);
```

When you run the above script, the **Alexa - Top sites** page will be printed to the console. And that's great. It means that we successfully parsed the HTML and collected the text of the `<title>` element from it using Cheerio. Let's break it down.

First, we loaded the downloaded `html` into `cheerio` using the `load()` function. This created a new function, conventionally named `$`, which allows us to work with the loaded HTML. Then we called `$('title')`, which found the `<title>` element by using the `title` CSS selector. Finally, we collected the text from the element using the `.text()` function and printed it to the console.

> `$('title')` is very similar to calling `document.querySelector('title')` in the browser and `element.text()` is similar to `element.textContent` from the earlier DevTools chapters. [Visit the cheerio documentation](https://github.com/cheeriojs/cheerio#readme) to learn more about its syntax, which is the same as JQuery syntax.

## [](#next) Next up

In the next part of this chapter we will learn more about Cheerio and use it to collect all the websites' data from the Alexa Top sites index.