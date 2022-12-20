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

## Original Inspiration

*okito, SproutCore Blog, Oct 27th, 2008:*

> The Gmail team used an interesting technique to reduce their load time on the iPhone by loading scripts as comment which they later regexed and `eval`’d on demand. [...] Believe it or not, your browser can actually spend a significant amount of time just parsing your code; especially if you have a lot of it.

> You can get around this with some trickery, it turns out, by downloading your JavaScript, not as actual script, but as something else.  The Gmail team wrapped their scripts in comments. [... Since...] comments both don’t involve constructing actual code, the browser is able to process this code much faster on initial download.  Only later, when you actually need the code, do you need to `eval()` the code to get it ready.

> Overall, wrapping code as a large comment is faster than [...] loading the function directly.  That is a huge win if parsing time is your primary bottleneck.

**Source:** https://web.archive.org/web/20091130185400/http://blog.sproutcore.com/post/225219087/faster-loading-through-eval
