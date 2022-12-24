[Link](https://www.codewars.com/kata/55c04b4cc56a697bb0000048) #Strings #Performance #Algorithms

Complete the function `scramble(str1, str2)` that returns `true` if a portion of `str1` characters can be rearranged to match `str2`, otherwise returns `false`.

**Notes:**

-   Only lower case letters will be used (a-z). No punctuation or digits will be included.
-   Performance needs to be considered.

## Examples

```python
scramble('rkqodlw', 'world') ==> True
scramble('cedewaraaossoqqyt', 'codewars') ==> True
scramble('katas', 'steak') ==> False
```

***
# Solutions
#js
```js
function scramble(str1, str2) {
  let str1Chars = {}
  let str2Chars = {}
  
  for (let char of str1) {
    if (!str1Chars[char]) str1Chars[char] = 0
    str1Chars[char] += 1
  }
  
  for (let char of str2) {
    if (!str2Chars[char]) str2Chars[char] = 0
    str2Chars[char] += 1
  }
  
  for (let itemKey in str2Chars) {
    if (str2Chars[itemKey] > (str1Chars[itemKey] ?? 0)) return false
  }
  
  return true
}
```