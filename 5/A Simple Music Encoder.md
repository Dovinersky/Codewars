[Link](https://www.codewars.com/kata/58db9545facc51e3db00000a) #Algorithms

You have been hired by a major MP3 player manufacturer to implement a new music compression standard. In this kata you will implement the ENCODER, and its companion kata deals with the [DECODER](https://www.codewars.com/kata/a-simple-music-decoder). It can be considered a harder version of [Range Extraction](https://www.codewars.com/kata/range-extraction).

# Specification

The input signal is represented as an array of integers. Several cases of regularities can be shortened.

-   A sequence of 2 or more identical numbers is shortened as `number*count`
-   A sequence of 3 or more consecutive numbers is shortened as `first-last`. This is true for both ascending and descending order
-   A sequence of 3 or more numbers with the same interval is shortened as `first-last/interval`. Note that the interval does NOT need a sign
-   Compression happens left to right

# Examples

-   `[1, 3, 4, 5, 7, 8, 9, 10, 11, 14, 15, 17, 18, 19, 20]` is compressed to `"1,3-5,7-11,14,15,17-20"`
-   `[0, 2, 4, 5, 7, 8, 9]` is compressed to `"0-4/2,5,7-9"`
-   `[0, 2, 4, 5, 7, 6, 5]` is compressed to `"0-4/2,5,7-5"`
-   `[0, 2, 4, 5, 7, 6, 5, 5, 5, 5, 5]` is compressed to `"0-4/2,5,7-5,5*4"`

# Input

A non-empty array of integers

# Output

A string of comma-separated integers and sequence descriptors

***
# Solutions
#ts 
```ts
export function compress(input: number[]): string {
    let tokens: string[] = [];
    let differences: number[] = [];
    for (let i = 1; i < input.length; i++) differences.push(input[i - 1] - input[i]);

    let sequenceDifference: number | null = null;
    let index = 0;
    let exitRequired = false;
    let initSequenceInfo = () => {
        sequenceDifference = null;
        index = 0;
    };

    while (!exitRequired) {
        if (index > differences.length - 1) exitRequired = true;

        if (sequenceDifference == null) {
            sequenceDifference = differences[index];
            index++;
            continue;
        }
        if (sequenceDifference == differences[index]) {
            index++;
            continue;
        }

        // count >= 2, diff = 0
        if (sequenceDifference == 0) {
            tokens.push(`${input[0]}*${index + 1}`);
            input = input.slice(index + 1);
            differences = differences.slice(index + 1);
            initSequenceInfo();
            continue;
        }

        // count = 1, diff = any
        if (index == 1) {
            tokens.push(`${input.shift()}`);
            differences.shift();
            initSequenceInfo();
            continue;
        }

        // count >= 2, diff > 0
        if (index >= 2) {
            let absDifference = Math.abs(sequenceDifference);
            tokens.push(`${input[0]}-${input[index]}${absDifference == 1 ? "" : `/${absDifference}`}`);
            input = input.slice(index + 1);
            differences = differences.slice(index + 1);
            initSequenceInfo();
            continue;
        }

        // count = 2, diff > 0
        tokens.push(`${input.shift()}`);
        differences.shift();
        initSequenceInfo();
    }

    // if (input.length > 0) tokens.concat(input.map(String)).join(",");
    return tokens.concat(input.map(String)).join(",");
}
```