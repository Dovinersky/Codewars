[Link](https://www.codewars.com/kata/514b92a657cdc65150000006) #Mathematics #Algorithms

If we list all the natural numbers below 10 that are multiples of 3 or 5, we get 3, 5, 6 and 9. The sum of these multiples is 23.

Finish the solution so that it returns the sum of all the multiples of 3 or 5 **below** the number passed in. Additionally, if the number is negative, return 0 (for languages that do have them).

**Note:** If the number is a multiple of **both** 3 and 5, only count it _once_.

_**Courtesy of projecteuler.net** ([Problem 1](https://projecteuler.net/problem=1))_

***
# Solutions
#ts 
```ts
export class Challenge {
    static solution(number: number) {
        if (number-- < 4) return 0;
        let result = 0;
        new Set([
            ...[...Array(Math.floor(number / 3))].map((item, index) => 3 + 3 * index),
            ...[...Array(Math.floor(number / 5))].map((item, index) => 5 + 5 * index)
        ]).forEach((item) => (result += item));
        return result;
    }
}
```