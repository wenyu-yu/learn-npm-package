## 1. decamelize

> Convert a camelized string into a lowercased one with a custom separator
> Example: `unicornRainbow` → `unicorn_rainbow`

```javascript
const decamelize = require('decamelize');

decamelize('unicornRainbow');        //=> 'unicorn_rainbow'

decamelize('unicornRainbow', '-');   //=> 'unicorn-rainbow'
```



## 2. pad-left

> Left pad a string with zeros or a specified string. Fastest implementation.

```javascript
var pad = require('pad-left');
pad(  '4', 4, '0') // 0004
pad( '35', 4, '0') // 0035
pad('459', 4, '0') // 0459
```

```javascript
var repeat = require('repeat-string');

module.exports = function padLeft(str, num, ch) {
  str = str.toString();

  if (typeof num === 'undefined') {
    return str;
  }

  if (ch === 0) {
    ch = '0';
  } else if (ch) {
    ch = ch.toString();
  } else {
    ch = ' ';
  }

  return repeat(ch, num - str.length) + str;
};
```



## 3. repeat-string

> Repeat the given string n times. Fastest implementation for repeating a string.

```javascript
var repeat = require('repeat-string');
repeat('A', 5);  //=> AAAAA
```



## 4. to-camel-case

```javascript
var toCamelCase = require('to-camel-case')

toCamelCase('space case')  // "spaceCase"
toCamelCase('snake_case')  // "snakeCase"
toCamelCase('dot.case')    // "dotCase"
toCamelCase('weird[case')  // "weirdCase"
```

```javascript
var space = require('to-space-case')
function toCamelCase(string) {
  return space(string).replace(/\s(\w)/g, function (matches, letter) {
    return letter.toUpperCase()
  })
}
```



## 5. to-space-case

```javascript
var toSpaceCase = require('to-space-case')

toSpaceCase('camelCase')             // "camel case"
toSpaceCase('snake_case')            // "snake case"
toSpaceCase('dot.case')              // "dot case"
toSpaceCase('-RAnDom -jUNk$__loL!')  // "random junk lol"
```

```javascript
var clean = require('to-no-case')

module.exports = function toSpaceCase(string) {
  return clean(string).replace(/[\W_]+(.|$)/g, function (matches, match) {
    return match ? ' ' + match : ''
  }).trim()
}
```



## 6. to-no-case

> Remove any existing casing from a string

```javascript
var toNoCase = require('to-no-case')

toNoCase('camelCase')            // "camel case"
toNoCase('snake_case')           // "snake case"
toNoCase('slug-case')            // "slug case"
toNoCase('Title of Case')        // "title of case"
toNoCase('Sentence case.')       // "sentence case."
toNoCase('RAnDom -jUNk$__loL!')  // "random -junk$__lol!"
```



## 7. to-capital-case

```javascript
var toCapitalCase = require('to-capital-case')

toCapitalCase('camelCase')        // "Camel Case"
toCapitalCase('space case')       // "Space Case"
toCapitalCase('snake_case')       // "Snake Case"
toCapitalCase('dot.case')         // "Dot Case"
toCapitalCase('some*weird[case')  // "Some Weird Case"
```

```javascript
var space = require('to-space-case')

module.exports = function toCapitalCase(string) {
  return space(string).replace(/(^|\s)(\w)/g, function (matches, previous, letter) {
    return previous + letter.toUpperCase()
  })
}
```



## 8. to-pascal-case

```javascript
var toPascalCase = require('to-pascal-case')

toPascalCase('space case')  // "SpaceCase"
toPascalCase('snake_case')  // "SnakeCase"
toPascalCase('dot.case')    // "DotCase"
toPascalCase('weird[case')  // "WeirdCase"
```

```javascript
var space = require('to-space-case')

module.exports = function toPascalCase(string) {
  return space(string).replace(/(?:^|\s)(\w)/g, function (matches, letter) {
    return letter.toUpperCase()
  })
}
```



## 9. to-title-case

```javascript
var toTitleCase = require('to-title-case')

toTitleCase('the catcher in the rye') 
// "The Catcher in the Rye"
```



## 10. to-sentence-case

```javascript
var toSentenceCase = require('to-sentence-case')

toSentenceCase('the catcher, in the rye.') 
// "The catcher, in the rye."

```

```javascript
var clean = require('to-no-case')
function toSentenceCase(string) {
  return clean(string).replace(/[a-z]/i, function (letter) {
    return letter.toUpperCase()
  }).trim()
}
```



## 11. strip-ansi

> Strip [ANSI escape codes](https://en.wikipedia.org/wiki/ANSI_escape_code) from a string

```javascript
const stripAnsi = require('strip-ansi');

stripAnsi('\u001B[4mUnicorn\u001B[0m');
//=> 'Unicorn'

stripAnsi('\u001B]8;;https://github.com\u0007Click\u001B]8;;\u0007');
//=> 'Click'
```