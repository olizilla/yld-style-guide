# YLD Javascript style guide

```javascript
var thisIsJavascript = "no camel_case please";
var beConsistentFfs = "TitleCase Acronyms, don't do a XMLHttpRequest";

var oneVarPerLine = "it keeps git history clean, and that's important";

var Yld = { // object literals, not `new Object` obvs
  type: "company", // don't needlessly 'quote' keys
  location: "everywhere!", // trailing commas are good: less git noise when adding/rearranging keys. we write node so it has no legacy issues
}; // use semi-colons. You know how to omit them, but people maintaing your code might not :)

var technologies = ["node", "docker", "ansible", "erlang?!"]; // not `new Array`, obvs
```

## Functions

```javascript
demonstrateFunctions("Isn't it great that I can call this above the declaration in the same scope?");

// prefer function declarations where possible, they enable you to order your code
// from the highest level (big picture) to lowest level (tiny boring helpers) which is
// much more readable. and they have proper names for stack traces, yay!
function demonstrateFunctions(msg) {
  console.log(msg);
}
```

## Comparisons

```javascript
var x = null;

var notNullOrUndefined = x == null; // this is the only good use for `==`, avoiding the ugly `x === undefined || x === null`

if(x === 0) {
  // otherwise stick to triple equals, and everyone will know what you're trying to do!
}
```

## Control flow

```javascript
function returnEarlyIfPossible() {
  // suggest people always use braces with control-flow. Noisier but it does make things explicit.
  if(x) {
    return true;
  }
}

// please never ever use logical operators for control flow! e.g `x && x.explode()`
```

## Object-orientation

Use OO when you have things with state and a large API. As far as possible KISS - functions and data are very predicatable and flat, object hierarchies are not.

```javascript
// PascalCaseConstructors
function YlderAgent(name) {
  this.name = name;
  
  // private fields / methods are prefixed with `_`. 
  // we use them to tell people they're internal implementation details
  // and are subject to API change without warning
  this._lessons = 0; 
}

// use prototypes for cases where you'll be making lots of instances
// to avoid waste. if it's not used loads, feel free to use another pattern (just keep it readable).
YlderAgent.prototype = {
  ranTraining: function() {
    this._lessons += 1;
  },
  // yes, v8 is ES5 so feel free to use getters/setters (where appropriate, don't surprise ppl!)
  get lessons() {
    return this._lessons;
  },
}
YlderAgent.prototype.constructor = YlderAgent;

var sarah = new YlderAgent("sarah");
sarah.ranTraining();
sarah.ranTraining();
```

## Callbacks

Keep work and orchestration separate. Test work separately.

If things get really complex, use `async`.
