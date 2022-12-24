[Link](https://www.codewars.com/kata/58693136b98de0e4910001ab) #Strings #Fundamentals

You'll be given a string of random characters (numbers, letters, and symbols). To decode this string into the key we're searching for:

(1) count the number occurences of each ascii lowercase letter, and

(2) return an ordered string, 26 places long, corresponding to the number of occurences for each corresponding letter in the alphabet.

For example:

```python
'$aaaa#bbb*cc^fff!z' gives '43200300000000000000000001'
   ^    ^   ^  ^  ^         ^^^  ^                   ^
  [4]  [3] [2][3][1]        abc  f                   z
  
'z$aaa#ccc%eee1234567890' gives '30303000000000000000000001'
 ^  ^   ^   ^                    ^ ^ ^                    ^
[1][3] [3] [3]                   a c e                    z
```

Remember, the string returned should always be 26 characters long, and only count lowercase letters.

Note: You can assume that each lowercase letter will appears a maximum of 9 times in the input string.

***
# Solutions
#js 
```js
function decrypt(encryption) {
  // a = 97 z = 122
  let offset = 97
  let result = Array.from({length: 26}, (element, index) => { return 0 })
  let banches = encryption.split(/[^a-z]/)

  banches.forEach(str => {
    for (let char of str) result[char.charCodeAt(0) - offset]++
  })

  return result.join('')
}
```