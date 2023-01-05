[Link](https://www.codewars.com/kata/55983863da40caa2c900004e) #Strings #Refactoring

Create a function that takes a positive integer and returns the next bigger number that can be formed by rearranging its digits. For example:

```
12 ==> 21
513 ==> 531
2017 ==> 2071
```

```swift
nextBigger(num: 12)   // returns 21
nextBigger(num: 513)  // returns 531
nextBigger(num: 2017) // returns 2071
```

If the digits can't be rearranged to form a bigger number, return `-1` (or `nil` in Swift):

```
9 ==> -1
111 ==> -1
531 ==> -1
```

```swift
nextBigger(num: 9)   // returns nil
nextBigger(num: 111) // returns nil
nextBigger(num: 531) // returns nil
```

***
# Solutions
#cs 
```cs
using System.Linq;
using System.Collections.Generic;

public class Kata
{
    public static long NextBiggerNumber(long input)
    {
        List<int> digits = input.ToString().Select(charDigit => charDigit - '0').ToList();
        List<int> constantSequence = null;
        List<int> sequence = null;

        // Find the last pair of digits in ascending order.
        // Use the pair as indicator to split input digits sequence:
        // 1. First constant list contains digits preciding the pair (excluding),
        // 2. Second list containts the pair and remainig digits.
        // Ex: 1238746871 -> 123874[68]71 -> 123874 and 6871.
        for (int i = digits.Count - 1; i > 0; i--)
            if (digits[i - 1] < digits[i])
            {
                constantSequence = digits.Take(i - 1).ToList();
                sequence = digits.Skip(i - 1).ToList();
                break;
            }

        // Empty sequence means there are no any pairs in ascending order within input sequence
        // and there cannot be any permutations to get bigger number.
        if (sequence == null) return -1;

        // Sorted sequence copy is required to find
        // the closest bigger digit to smaller value in the pair.
        // The smaller pair digit always has index 0.
        // Ex: 6871 -> index 0 digit is 6; next bigger is 7.
        List<int> sortedSequence = sequence.ToArray().ToList();
        sortedSequence.Sort();

        int firstSequenceDigit = sequence[0];
        int nextBiggerDigit = sortedSequence.Find(digit => digit > firstSequenceDigit);
        int nextBiggerDigitIndex = sequence.FindLastIndex(digit => digit == nextBiggerDigit);

        // Swap first digit and closest bigger digit.
        // Ex: 6871 -> 7861.
        // Sort sequence excluding first digit.
        // Ex: 7861 -> 7168.
        sequence[0] = sequence[nextBiggerDigitIndex];
        sequence[nextBiggerDigitIndex] = firstSequenceDigit;

        sortedSequence = sequence.Skip(1).ToList();
        sortedSequence.Sort();
        sequence = sequence.Take(1).Concat(sortedSequence).ToList();

        // Concatenate constant sequence from the beginning and sorted sequence.
        // 123874 + 7168
        return long.Parse(constantSequence == null ? "" : string.Join("", constantSequence) + string.Join("", sequence));
    }
}
```