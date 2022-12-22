# Reveal Hidden Scripts
Reveal Hidden Scripts is a technique in which JavaScript functions are wrapped in JS comments and later unwrapped, parsed and executed.

At this stage this feels like a solution looking for a problem...

*ie. this is still a proof of concept which may (at some point) find utility - or it may not ever.*

_______

## External Javascript File:

```js

/*¤¤CONSOLE_LOG_1¤¤ console.log('This test is really working!'); ¤¤¤*/

/*¤¤CONSOLE_LOG_2¤¤ console.log('This test is also really working!!'); ¤¤¤*/

/*¤¤CHANGE_MAIN_HEADING_COLOR¤¤ document.querySelector('h1').style.setProperty('color', 'pink'); ¤¤¤*/

```

_______

## Main Javascript File:

```js

  //***********************//
 // REVEAL HIDDEN SCRIPTS //
//***********************//

function revealHiddenScript(hiddenScript, hiddenScriptNames = []) {

  for (let hiddenScriptName of hiddenScriptNames) {

    let hiddenScriptNameRegex = new RegExp('\\/\\*¤¤' + hiddenScriptName + '¤¤\\s([^¤]+)¤¤¤\\*\\/');
    let hiddenScriptExcerpt = hiddenScript.match(hiddenScriptNameRegex)[1];
    Function(`${hiddenScriptExcerpt}`)();
  }
}

let hiddenScriptNames = ['CONSOLE_LOG_1', 'CONSOLE_LOG_2', 'CHANGE_MAIN_HEADING_COLOR'];
let hiddenScriptURL = '/external-javascript-file.js';
requestRemoteResponse(hiddenScriptURL, revealHiddenScript, hiddenScriptNames);

```
_____

## Proof of Concept

When the **External Javascript File** above is expanded 1000 times, the resulting file is `276.6KB`.

The same file, with all comments removed (including the capitalised labels), is `162.3KB`.

When parsed by Carlos Bueno's [**Parse 'n' Load** Tool](https://github.com/aristus/parse-n-load), the file without comments parses and loads in an average time of: `135ms`

By contrast the entirely commented file parses and loads in an average time of: `34ms`

This reveals that the *commented script file* - not less than 170.4% the size of its counterpart - may be parsed in ***a quarter*** the time it takes for the *uncommented script file* to be parsed.

For what it's worth, this shows that having a script completely commented out when it is first downloaded saves a significant amount of time.

Here we are saving as much as **75%** of the time it takes to download and parse a JS file.

Since anything above 100ms is noticeable (while anything below is perceived as near-instantaneous) reducing processing time from `135ms` to `34ms` is a dramatic difference. 

_____

## Original Inspiration

*okito, SproutCore Blog, Oct 27th, 2008:*

> The Gmail team used an interesting technique to reduce their load time on the iPhone by loading scripts as comment which they later regexed and `eval`’d on demand. [...] Believe it or not, your browser can actually spend a significant amount of time just parsing your code; especially if you have a lot of it.

> You can get around this with some trickery, it turns out, by downloading your JavaScript, not as actual script, but as something else.  The Gmail team wrapped their scripts in comments. [... Since...] comments both don’t involve constructing actual code, the browser is able to process this code much faster on initial download.  Only later, when you actually need the code, do you need to `eval()` the code to get it ready.

> Overall, wrapping code as a large comment is faster than [...] loading the function directly.  That is a huge win if parsing time is your primary bottleneck.

**Source:** https://web.archive.org/web/20091130185400/http://blog.sproutcore.com/post/225219087/faster-loading-through-eval
