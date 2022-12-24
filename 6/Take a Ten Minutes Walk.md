[Link](https://www.codewars.com/kata/54da539698b8a2ad76000228) #Arrays #Fundamentals

You live in the city of Cartesia where all roads are laid out in a perfect grid. You arrived ten minutes too early to an appointment, so you decided to take the opportunity to go for a short walk. The city provides its citizens with a Walk Generating App on their phones -- everytime you press the button it sends you an array of one-letter strings representing directions to walk (eg. ['n', 's', 'w', 'e']). You always walk only a single block for each letter (direction) and you know it takes you one minute to traverse one city block, so create a function that will return **true** if the walk the app gives you will take you exactly ten minutes (you don't want to be early or late!) and will, of course, return you to your starting point. Return **false** otherwise.

> **Note**: you will always receive a valid array containing a random assortment of direction letters ('n', 's', 'e', or 'w' only). It will never give you an empty array (that's not a walk, that's standing still!).

***
# Solutions
#js 
```js
function isValidWalk(walk) {
  if (walk.length != 10) return false
  
  let coord = { x: 0, y: 0 }
  walk.reduce((acc, elem) => {
    switch (elem) {
      case 'n': acc.y -= 1; break;
      case 'e': acc.x += 1; break;
      case 's': acc.y += 1; break;
      case 'w': acc.x -= 1; break;
    }
    return acc
  }, coord)

  if (coord.x == 0 && coord.y == 0) return true
  return false
}
```