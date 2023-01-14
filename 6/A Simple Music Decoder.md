[Link](https://www.codewars.com/kata/58de42bab4b74c214d0000e2) #Algorithms

You have been hired by a major MP3 player manufacturer to implement a new music compression standard. In this kata you will implement the DECODER, and its companion kata deals with the [ENCODER](https://www.codewars.com/kata/a-simple-music-encoder).

# Specification

The input signal is represented as a comma-separated string of integers and tokens (sequence descriptors). Tokens must be expanded as follows.

-   `number*count` is expanded as `number` repeated `count` times
-   `first-last` is expanded as a sequence of consecutive numbers starting with `first` and ending with `last`. This is true for both ascending and descending order
-   `first-last/interval` is similarly expanded, as sequence starting with `first`, ending with `last`, where the numbers are separated by `interval`. Note that interval does NOT have a sign

# Examples

-   `"1,3-5,7-11,14,15,17-20"` is expanded to `[1, 3, 4, 5, 7, 8, 9, 10, 11, 14, 15, 17, 18, 19, 20]`
-   `"0-4/2, 5, 7-9"` is expanded to `[0, 2, 4, 5, 7, 8, 9]`
-   `"0-4/2, 5, 7-5"` is expanded to `[0, 2, 4, 5, 7, 6, 5]`
-   `"0-4/2, 5, 7-5, 5*4"` is expanded to `[0, 2, 4, 5, 7, 6, 5, 5, 5, 5, 5]`

# Input

A string of comma-separated integers and tokens (sequence descriptors)

# Output

An array of integers

***
# Solutions
#ts
```ts
export function uncompress(music: string): number[] {
    return music.split(/,\s?/).reduce((acc, item) => {
        let splitted: string[] = item.split(/\*/);

        // Number with counter
        if (splitted.length > 1) return acc.concat(Array(Number(splitted[1])).fill(Number(splitted[0])));
        
        // Just a number
        if (!splitted[0].match(/\d-/)) return acc.concat(Number(splitted[0]));

        // Range
        splitted = item.split(/\//);
        let step: number = splitted.length > 1 ? Number(splitted[1]) : 1;

        let splittedArray: string[] = splitted[0].split("");
        splittedArray[splitted[0].search(/\d-/) + 1] = "a";
        splitted = splittedArray.join("").split(/a/);

        let left: number = Number(splitted[0]);
        let right: number = Number(splitted[1]);

        // Range in ascending order
        if (left < right)
            return acc.concat(
                Array((right - left) / step + 1)
                    .fill(0)
                    .map((item, index) => left + index * step)
            );

        // Range in descending order
        return acc.concat(
            Array((left - right) / step + 1)
                .fill(0)
                .map((item, index) => left - index * step)
        );
    }, <number[]>[]);
}
```