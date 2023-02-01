[Link](https://www.codewars.com/kata/51c8e37cee245da6b40000bd) #Strings #Algorithms

Complete the solution so that it strips all text that follows any of a set of comment markers passed in. Any whitespace at the end of the line should also be stripped out.

**Example:**

Given an input string of:

```
apples, pears # and bananas
grapes
bananas !apples
```

The output expected would be:

```
apples, pears
grapes
bananas
```

The code would be called like so:

```javascript
var result = solution("apples, pears # and bananas\ngrapes\nbananas !apples", ["#", "!"])
// result should == "apples, pears\ngrapes\nbananas"
```

***
# Solutions
#js 
```js
let solution = (input, markers) =>
  input.split("\n").map((token) => {
    let splitToken = [];
    markers.some((marker) => {
      splitToken = token.split(new RegExp(marker === "$" ? /\$/ : marker))
      if (splitToken.length > 1) return true;
      return false;
    })
    return splitToken[0].trimEnd();
  }).join("\n");
```